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
<style>
  .scrollable-code {
  max-height: 300px; /* Set the desired max height */
  overflow-y: auto;  /* Enable vertical scrolling */
  overflow-x: hidden; /* Disable horizontal scrolling (optional) */
  /* background-color: #f8f8f8; Matches the code block's background */
  padding: 10px;     /* Optional: Adjusts the padding */
  max-width: 300px;
  border: 1px solid #ccc; /* Optional: Adds a border for aesthetics */
}
</style>

### Overview

<!-- Anyways I hope you enjoy this four part walkthrough. I learned a lot putting it together, especially how to spell the word 'possession'. -->

This page will be an informational summary followed by a walkthrough of the _SlidingWindowNFL-1_ file.

# Informational Summary

### Data layout & structure
The input data contains a list of game data between the home and visitor team.

<div class="scrollable-code">
<table>
  <thead>
    <tr>
      <th>Variable</th>
      <th>Data Type</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Season</td><td>int64</td></tr>
    <tr><td>Date</td><td>object</td></tr>
    <tr><td>Home_Team</td><td>object</td></tr>
    <tr><td>H_Q1</td><td>int64</td></tr>
    <tr><td>H_Q2</td><td>int64</td></tr>
    <tr><td>H_Q3</td><td>int64</td></tr>
    <tr><td>H_Q4</td><td>int64</td></tr>
    <tr><td>H_OT</td><td>int64</td></tr>
    <tr><td>H_Final</td><td>int64</td></tr>
    <tr><td>Visitor_Team</td><td>object</td></tr>
    <tr><td>V_Q1</td><td>int64</td></tr>
    <tr><td>V_Q2</td><td>int64</td></tr>
    <tr><td>V_Q3</td><td>int64</td></tr>
    <tr><td>V_Q4</td><td>int64</td></tr>
    <tr><td>V_OT</td><td>int64</td></tr>
    <tr><td>V_Final</td><td>int64</td></tr>
    <tr><td>H_First_Downs</td><td>int64</td></tr>
    <tr><td>V_First_Downs</td><td>int64</td></tr>
    <tr><td>H_Rush</td><td>int64</td></tr>
    <tr><td>V_Rush</td><td>int64</td></tr>
    <tr><td>H_Yds</td><td>int64</td></tr>
    <tr><td>V_Yds</td><td>int64</td></tr>
    <tr><td>H_TDs</td><td>int64</td></tr>
    <tr><td>V_TDs</td><td>int64</td></tr>
    <tr><td>H_Cmp</td><td>int64</td></tr>
    <tr><td>V_Cmp</td><td>int64</td></tr>
    <tr><td>H_Att</td><td>int64</td></tr>
    <tr><td>V_Att</td><td>int64</td></tr>
    <tr><td>H_Yd</td><td>int64</td></tr>
    <tr><td>V_Yd</td><td>int64</td></tr>
    <tr><td>H_TD</td><td>int64</td></tr>
    <tr><td>V_TD</td><td>int64</td></tr>
    <tr><td>H_INT</td><td>int64</td></tr>
    <tr><td>V_INT</td><td>int64</td></tr>
    <tr><td>H_Sacked</td><td>int64</td></tr>
    <tr><td>V_Sacked</td><td>int64</td></tr>
    <tr><td>H_Sacked_Yards</td><td>int64</td></tr>
    <tr><td>V_Sacked_Yards</td><td>int64</td></tr>
    <tr><td>H_Net_Pass_Yards</td><td>int64</td></tr>
    <tr><td>V_Net_Pass_Yards</td><td>int64</td></tr>
    <tr><td>H_Total_Yards</td><td>int64</td></tr>
    <tr><td>V_Total_Yards</td><td>int64</td></tr>
    <tr><td>H_Fumbles</td><td>int64</td></tr>
    <tr><td>V_Fumbles</td><td>int64</td></tr>
    <tr><td>H_Lost</td><td>int64</td></tr>
    <tr><td>V_Lost</td><td>int64</td></tr>
    <tr><td>H_Turnovers</td><td>int64</td></tr>
    <tr><td>V_Turnovers</td><td>int64</td></tr>
    <tr><td>H_Penalties</td><td>int64</td></tr>
    <tr><td>V_Penalties</td><td>int64</td></tr>
    <tr><td>H_Penalties_Yards</td><td>int64</td></tr>
    <tr><td>V_Penalties_Yards</td><td>int64</td></tr>
    <tr><td>H_Third_Down_Conv</td><td>object</td></tr>
    <tr><td>V_Third_Down_Conv</td><td>object</td></tr>
    <tr><td>H_Fourth_Down_Conv</td><td>object</td></tr>
    <tr><td>V_Fourth_Down_Conv</td><td>object</td></tr>
    <tr><td>H_Time_of_Possession</td><td>object</td></tr>
    <tr><td>V_Time_of_Possession</td><td>object</td></tr>
    <tr><td>H_passing_att</td><td>float64</td></tr>
    <tr><td>H_passing_cmp</td><td>float64</td></tr>
    <tr><td>H_passing_int</td><td>float64</td></tr>
    <tr><td>H_passing_lng</td><td>float64</td></tr>
    <tr><td>H_passing_sk</td><td>float64</td></tr>
    <tr><td>H_passing_td</td><td>float64</td></tr>
    <tr><td>H_passing_yds</td><td>float64</td></tr>
    <tr><td>H_receiving_lng</td><td>float64</td></tr>
    <tr><td>H_receiving_td</td><td>float64</td></tr>
    <tr><td>H_receiving_yds</td><td>float64</td></tr>
    <tr><td>H_rushing_att</td><td>float64</td></tr>
    <tr><td>H_rushing_lng</td><td>float64</td></tr>
    <tr><td>H_rushing_td</td><td>float64</td></tr>
    <tr><td>H_rushing_yds</td><td>float64</td></tr>
    <tr><td>V_passing_att</td><td>float64</td></tr>
    <tr><td>V_passing_cmp</td><td>float64</td></tr>
    <tr><td>V_passing_int</td><td>float64</td></tr>
    <tr><td>V_passing_lng</td><td>float64</td></tr>
    <tr><td>V_passing_sk</td><td>float64</td></tr>
    <tr><td>V_passing_td</td><td>float64</td></tr>
    <tr><td>V_passing_yds</td><td>float64</td></tr>
    <tr><td>V_receiving_lng</td><td>float64</td></tr>
    <tr><td>V_receiving_td</td><td>float64</td></tr>
    <tr><td>V_receiving_yds</td><td>float64</td></tr>
    <tr><td>V_rushing_att</td><td>float64</td></tr>
    <tr><td>V_rushing_lng</td><td>float64</td></tr>
    <tr><td>V_rushing_td</td><td>float64</td></tr>
    <tr><td>V_rushing_yds</td><td>float64</td></tr>
    <tr><td>H_Final_Allowed</td><td>int64</td></tr>
    <tr><td>V_Final_Allowed</td><td>int64</td></tr>
    <tr><td>H_Won</td><td>float64</td></tr>
  </tbody>
</table>
</div>

<br /><br />


<h3>We transform the dataset into one that is useful for making predictions by:</h3>

1\. Defining the Y values
: This will be assigned as which team won the current game. Represented under the H_won column
: {% highlight python %}
# H_Won column
df['H_Won'] = np.where(df['H_Final'] > df['V_Final'], 1.0, 0.0)
{% endhighlight %}


2\. Applying an exponential moving average
: This is applied to most columns. For predicting game _n_ of a season, we use game data from the first game to the _n-1_ game (where _n_ is the number of games performed by the team during the season). Then we apply a minimum window of 4. The table below shows a visual example:<br /><br />


| Game | Data before EMA | EMA (ema_span=min(7, input data length)) |
|------|-----------------|-----------------------------------------|
| 1    | 436             | 436.000000                             |
| 2    | 489             | 466.285714                             |
| 3    | 363             | 421.621622                             |
| 4    | 416             | 419.565714                             |
| 5    | 311             | 383.979513                             |
| 6    | 269             | 349.010989                             |
| 7    | 257             | 322.464746                             |
| 8    | 286             | **312.334379**                         |


<br />
: In this table, the bold row item would be used as an input to calculate 

: We use pandas.DataFrame.ewm with the following setup:
{% highlight python %}
ema_span = 7   # To place influence on the past 7 games
ema = dataframe_val['value'].ewm(span=min(ema_span, len(home_col_list)), adjust=True).mean().iloc[-1]
{% endhighlight %}
<br /><br />
: Here is a [helpful guide](https://medium.com/@whyamit404/understanding-pandas-ewm-what-it-does-and-why-its-useful-f4d0f7f0334d) to the different parameters that can be adjusted for pandas ewm function. I applied this based on best judgement and wanted to avoid placing too much weight on more recent games since this is being applied across many columns and the data might be highly variable.

3\. Reducing columns between the home & visitor team into difference columns.
: Currently, the model will have a hard time understanding the relationship between each set of columns. For a small dataset of around 5,000 records, the model won't understand that we are passing in pairs of columns and for which team each column represents (ex: H_Yds & V_Yds where Yds represents the yards gained through running plays). To help the model more easily understand the relationship between each column, we will combine related columns by replacing two related columns into the difference between the home and visitor team. <br /><br />
: Using our _Yds example, if the home team has a greater amount of yards gained through running plays, the D_Yds variable will be a positive number. Otherwise, it will be 0 or negative. This adjustment reduces our total column count from around 100 to around 50


4\. Maintaining an ordered dataset
: Since we are dealing with time series data, we cannot apply stratified K-fold cross validation ensuring that all classes are accounted for. This isn't necessary to begin with since we can assume the two prediction classes (win or loss) are roughly equal in its own regard.

<br />
<br />
# Walkthrough of the important parts in _SlidingWindowNFL-1_

## 1. Create additional columns
Here we import the dataset and create additional columns
{% highlight python %}
# Create the H_Won (Y) column
df['H_Won'] = np.where(df['H_Final'] > df['V_Final'], 1.0, 0.0)

# Combine passing & rushing TD for UMAP
df['H_passing_rushing_td'] = df['H_passing_td'] + df['H_rushing_td']
df['V_passing_rushing_td'] = df['V_passing_td'] + df['V_rushing_td']

# Create the H_final_allowed, V_final_allowed columns
df['H_Final_Allowed'] = df['V_Final']
df['V_Final_Allowed'] = df['H_Final']
{% endhighlight %}


## 2. Check and remove missing data
This is explained in part 2 since it is apart of EDA. Around 30 rows with blanks were removed.

## 3. Feature engineering Time_of_Possession & *_Down_Conv
This is also explained in part 2 since it is apart of EDA. Three columns were converted into useable format for the 

## 4. Combine similar columns
This is explained in part 2 since it is apart of EDA. 

## 5. Create a dict to track the track_cols array
For each track_cols item on each team in each season, we compose `track_dict` with the keys being &lt;season&gt;\_&lt;team abbrv&gt;\_&lt;track_dict column&gt;. 
Using this dictionary, we can proceed to step 6 which reformats the _df_ variable to contain data formatted to the specifications required to train our model.<br /><br />

{% highlight python %}
track_dict = {}

for row in df.itertuples():
    # year = row.Date.split('-')[0]
    year = row.Season
    home_team = row.Home_Team
    visitor_team = row.Visitor_Team
    
    # Home or visitor team has < minimum_window total games
    for col in track_cols:
        home_column_name = f'{year}_{home_team}_{col}'
        visitor_column_name = f'{year}_{visitor_team}_{col}'
        
        # Home team
        home_col = col if col == 'Date' else 'H_' + col
        if home_column_name in track_dict:
            track_dict[home_column_name].append(getattr(row, home_col))
        else:
            track_dict[home_column_name] = [getattr(row, home_col)]
        
        # Visitor team
        visitor_col = col if col == 'Date' else 'V_' + col
        if visitor_column_name in track_dict:
            track_dict[visitor_column_name].append(getattr(row, visitor_col))
        else:
            track_dict[visitor_column_name] = [getattr(row, visitor_col)]
{% endhighlight %}
<br /><br /><br />


## 6. Use track_dict to reformat df
Here we do the following to reformat the df. <br />

Enforce minimum_window to update df or drop row
: Example: With `minimum_window=4`, if team one or team two in the 2001 season is on game n of the season, if n is &lt; minimum_window, then the game will be ignored. Otherwise, the data points will be used to retrieve an EMA of all of the previous games in the current season for both teams. This is used to calculatee most columns<br /><br />
: After df is composed, each row will contain data up to (not including) the current game<br /><br />
: This allows us to access the current data of game n taking into account all previous games not including game n. Knowing this, we can train a predictive model on each row without data leakage provided the data is offered sequentially.<br /><br />

Create custom columns
: This is covered in part 2, 

{% highlight python %}
minimum_window = 4
ema_span = 7
print(df.shape)

indices_to_drop = []
current_count = 0

for row in df.itertuples():
    if current_count % 300 == 0:
        print(f'{current_count}/{df.shape[0]}')
    current_count = current_count + 1
    index = row.Index
    # year = row.Date.split('-')[0]
    year = row.Season
    home_team = row.Home_Team
    visitor_team = row.Visitor_Team
    # Home team min window
    home_date_column = f'{year}_{home_team}_Date'
    visitor_date_column = f'{year}_{visitor_team}_Date'

    # Current row is older than Home team at min_window
    if len(track_dict[home_date_column]) > minimum_window and row.Date <= track_dict[home_date_column][minimum_window]:
        indices_to_drop.append(index)
        continue
    # Current row is older than Visitor team at min_window
    if len(track_dict[visitor_date_column]) > minimum_window and row.Date <= track_dict[visitor_date_column][minimum_window]:
        indices_to_drop.append(index)
        continue

    home_date_index = track_dict[home_date_column].index(row.Date)
    visitor_date_index = track_dict[visitor_date_column].index(row.Date)
    # print(f'H: {home_date_index} V: {visitor_date_index}')

    # Update df to have average for each track_cols (Ignoring 'Date', 'datediff' the 1-2nd item)
    for col in track_cols[1:]:
        if col in ['start_odds', 'halftime_odds']:
            continue
        else:
            # Update df to have average for each track_cols (Ignoring 'Date', 'datediff' the 1-2nd item)
            # Update home
            home_col_list = track_dict[f'{year}_{home_team}_{col}'][:home_date_index-1]
            dataframe_val = pd.DataFrame({'value': home_col_list})
            ema = dataframe_val['value'].ewm(span=min(ema_span, len(home_col_list)), adjust=True).mean().iloc[-1]
            df.at[index, 'H_' + col] = ema

            # Update Visitor
            visitor_col_list = track_dict[f'{year}_{visitor_team}_{col}'][:visitor_date_index-1]
            dataframe_val = pd.DataFrame({'value': visitor_col_list})
            ema = dataframe_val['value'].ewm(span=min(minimum_window, len(visitor_col_list)), adjust=False).mean().iloc[-1]
            df.at[index, 'V_' + col] = ema


    # --------------------------------------------------- 
    # ------------------ Custom Columns -----------------
    # --------------------------------------------------- 
    #
    
    # 1. Add variant of Bill James pythagorean expectation (NFL).
    # Recent games weighted more heavily since 'Final' columns not excluded from the above loop
    home_points_for = sum(track_dict[f'{year}_{home_team}_Final'][:home_date_index-1])
    home_points_against = sum(track_dict[f'{year}_{home_team}_Final_Allowed'][:home_date_index-1])
    df.at[index, 'H_pythagorean'] = home_points_for**2.37 / (home_points_for**2.37 + home_points_against**2.37)

    visitor_points_for = sum(track_dict[f'{year}_{visitor_team}_Final'][:visitor_date_index-1])
    visitor_points_against = sum(track_dict[f'{year}_{visitor_team}_Final_Allowed'][:visitor_date_index-1])
    df.at[index, 'V_pythagorean'] = visitor_points_for**2.37 / (visitor_points_for**2.37 + visitor_points_against**2.37)            
            
        
    # 2. Add num days since last game for home, visitor
    df.at[index, f'H_datediff'] = 0
    if home_date_index > 0:
        current_game_date = datetime.strptime(track_dict[home_date_column][home_date_index], "%Y-%m-%d")
        previous_game_date = datetime.strptime(track_dict[home_date_column][home_date_index-1], "%Y-%m-%d")
        game_diff = int((current_game_date - previous_game_date).days)
        # print(f'{current_game_date} minus {previous_game_date} is {game_diff}')
        df.at[index, f'H_datediff'] = game_diff
    
    df.at[index, f'V_datediff'] = 0
    if visitor_date_index > 0:
        current_game_date = datetime.strptime(track_dict[visitor_date_column][visitor_date_index], "%Y-%m-%d")
        previous_game_date = datetime.strptime(track_dict[visitor_date_column][visitor_date_index-1], "%Y-%m-%d")
        game_diff = int((current_game_date - previous_game_date).days)
        # print(f'{current_game_date} minus {previous_game_date} is {game_diff}')
        df.at[index, f'V_datediff'] = game_diff
        
df.drop(indices_to_drop, inplace=True)

# Add custom metrics to track_cols so it creates the difference (D_) column
track_cols.append('datediff')
track_cols.append('pythagorean')
for col in track_cols[1:]:
    cont_cols.append('D_' + col)
    df['D_' + col] = (df['H_' + col] - df['V_' + col]).round(3) # Round to 3 sig figs

print(track_cols)
track_cols.pop()


print(df.shape)
{% endhighlight %}
<br /><br /><br />



