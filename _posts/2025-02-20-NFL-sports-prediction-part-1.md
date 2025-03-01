---
layout: single
classes: wide
author_profile: disabled
title:  "NFL Sports Prediction - Part 1"
date:   2025-02-17 22:20:41 -0500
categories:
  - Machine Learning
permalink: "/NFL-part-1"
---

### Overview

Anyways I hope you enjoy this four part walkthrough. I learned a lot putting it together, especially how to spell the word 'possession'.

### Data layout & structure
The input data contains a list of game data between the home and visitor team.

We transform this dataset into one that is useful for making predictions by:
###### 1. Defining the Y values
This will be assigned as which team won the current game. Represented under the H_won column
{% highlight python %}
# H_Won column
df['H_Won'] = np.where(df['H_Final'] > df['V_Final'], 1.0, 0.0)
{% endhighlight %}


###### 2. Applying an exponential moving average
This will be applied to the majority of columns using game data from n-1 games (where _n_ is the number of games performed by the team during the season). This can be visualized in the following table:


| Data | EMA (ema_span=min(7, input data length)) |
|------|--------------|
| 436  | 436.000000   |
| 489  | 466.285714   |
| 363  | 421.621622   |
| 416  | 419.565714   |
| 311  | 383.979513   |
| 269  | 349.010989   |
| 257  | 322.464746   |
| 286  | **312.334379** |


We use pandas.DataFrame.ewm with the following setup:
{% highlight python %}
ema_span = 7   # To place influence on the past 7 games
ema = dataframe_val['value'].ewm(span=min(ema_span, len(home_col_list)), adjust=True).mean().iloc[-1]
{% endhighlight %}

Here is a [helpful guide](https://medium.com/@whyamit404/understanding-pandas-ewm-what-it-does-and-why-its-useful-f4d0f7f0334d) to the different parameters that can be adjusted for pandas ewm function. I applied this based on best judgement and wanted to avoid placing too much weight on more recent games since this is being applied across a large number of columns and the data might be highly variable.

###### 3. Reducing columns between the home & visitor team into difference columns.
Currently, the model will have a hard time inferring the relationship between the different sets of columns. For a dataset of around 5,000 records, it seems unreasonable to suggest that the model will understand that a strong majority of the 100 columns passed in are actually pairs of columns and for which team each column represents (ex: H_Yds & V_Yds where Yds represents the yards gained through running plays). To help the model more easily understand the relationship between each column, we will combine related columns by replacing two related columns into the difference between the home and visitor team. 

Using our _Yds variable, if the home team has a greater amount of yards gained through running plays, the D_Yds variable will be a positive number. Otherwise, it will be 0 or negative. This adjustment reduces our total column count from around 100 to around 50


### Maintaining an ordered dataset
Since we are dealing with time series data, we cannot apply stratified K-fold cross validation ensuring that all classes are accounted for. This isn't really necessary to begin with since we can assume the two prediction classes (win or loss) are








