---
layout: single
classes: wide
author_profile: false
sidebar:
  nav: NFL-part-2
title:  "NFL Sports Prediction Part 2: Exploratory Data Analysis via ydata_profiling"
date:   2025-02-17 22:20:41 -0500
categories:
  - Machine Learning
permalink: "/NFL-part-2"
---
<style>
  .scrollable-code {
  max-height: 300px; /* Set the desired max height */
  overflow-y: auto;  /* Enable vertical scrolling */
  overflow-x: hidden; /* Disable horizontal scrolling (optional) */
  background-color: #f8f8f8; /* Matches the code block's background */
  padding: 10px;     /* Optional: Adjusts the padding */
  border: 1px solid #ccc; /* Optional: Adds a border for aesthetics */
}
</style>

**Note** Although there is a dedicated file `NFL_EDA-2.ipynb` for this step of the process, some of the EDA is performed in the previous `SlidingWindow-1.ipynb` file.
{: .notice}



## Column definitions
{% highlight python %}
cont_cols = [
    'D_datediff',              # Days since last game (Home - visitor)
    
    # first downs
    'D_First_Downs',
    
    # Basic Stats
    'D_Rush',                  # Number of running plays attempted
    'D_Yds',                   # Yards gained through running plays
    'D_TDs',                   # Touchdowns scored via running plays
    'D_Cmp',                   # Completions (# of successful passes)
    'D_Att',                   # Attempts (# of passes thrown, completed or not)
    'D_Yd',                    # Yards (Yards the passes have covered)
    'D_TD',                    # Touchdowns
    'D_INT',                   # Interceptions
    'D_Sacked',                # Number of times quarterback was tackled behind line of scrimmage
    'D_Yards',                 # Yards lost from sacks
    'D_Net_Pass_Yards',        # Net passing yards (total yds - yards lost due to sacks)
    'D_Total_Yards',           # Total yards gained (net pass yards + rushing yds)
    'D_Fumbles',               # Number of times ball was fumbled
    'D_Lost',                  # Number of times the team lost possession of the ball due to a fumble
    'D_Turnovers',             # Total number of turnovers, includes interceptions & fumbles lost
    'D_Penalties',             # Number of penalties committed by the team
    
    # Passing Detailed
    'D_passing_att',           # Passes attempted
    'D_passing_cmp',           # Passes completed
    'D_passing_int',           # Interceptions thrown
    'D_passing_lng',           # Longest completed pass
    'D_passing_sk',            # Passing times sacked
    'D_passing_td',            # Passing touchdowns
    'D_passing_yds',           # Yards gained by passing
    
    # Receiving
    'D_receiving_lng',         # Longest reception
    'D_receiving_td',          # Receiving touchdowns
    'D_receiving_yds',         # Receiving yards
    
    # Rushing Detailed
    'D_rushing_att',           # Rushing attempts (sacks not included)
    'D_rushing_lng',           # Longest rushing attempt (sacks not included)
    'D_rushing_td',            # Rushing touchdowns
    'D_rushing_yds',           # Rushing yards
    
    # Defense interceptions
    'D_def_interceptions_int', # Passes intercepted on defense
    'D_def_interceptions_lng', # Longest interception returned
    'D_def_interceptions_td',  # Interceptions returned for touchdown
    'D_def_interceptions_yds', # Yards interceptions were returned
    
    # Defense fumbles
    'D_fumbles_ff',            # Num of times forced a fumble by the opposition recovered by either team
    'D_fumbles_fr',            # Fumbles recovered by player or team
    'D_fumbles_td',            # Fumbles recovered resulting in touchdown for receiver
    'D_fumbles_yds',           # Yards recovered fumbles were returned
    
    # Defense tackles
    'D_sk',                    # Sacks
    'D_tackles_ast',           # Assists on tackles
    'D_tackles_comb',          # Solo + ast tackles
    'D_tackles_solo',          # Tackles
    
    # Kick Returns
    'D_kick_returns_lng',      # Longest kickoff return
    'D_kick_returns_rt',       # Kickoff returns 
    'D_kick_returns_td',       # Kickoffs returned for a touchdown
    'D_kick_returns_yds',      # Yardage for kickoffs returned
    
    # Punt Returns
    'D_punt_returns_lng',      # Longest punt return
    'D_punt_returns_ret',      # Punts returned
    'D_punt_returns_td',       # Punts returned for touchdown
    'D_punt_returns_yds',      # Punts return yardage
    
    # Punting / Scoring
    'D_punting_lng',           # Longest punt
    'D_punting_pnt',           # Times punted
    'D_punting_yds',           # Total punt yardage
    'D_scoring_fga',           # Field goals attempted
    'D_scoring_fgm',           # Field goals made
    'D_scoring_xpa',           # Extra points attempted
    'D_scoring_xpm',           # Extra points made
    
    # Additional, calculated metrics
    'D_pythagorean',           # NFL variation of Bill James pythagorean expectation (from wikipedia)
]
{% endhighlight %}

### Handling categorical columns
There are no categorical columns to handle in this dataset.

### Missing values
We have to apply this to the data coming in before all of the transformations, so this is performed in the `SlidingWindow-1.ipynb` file. Overall, we found between 27 to 31 rows with missing values across a few different columns Third_Down_Conv, Fourth_Down_Conv, and Time_of_Possession. All of the other columns had no missing data. Since we obseved only a very small amount of missing values, we don't have to worry about missing games interfering with the overall prediction.
<br/><br/>





# Analysis of the correlation matrix
With this first y_data analysis, we hope to find ways to reduce the complexity of the dataset so that the model can more quickly learn and draw conclusions on the data with as little noise as possible. Some of this will be based on the analysis, other changes will be based on domain knowledge & my best judgement. Because of the size of the dataset, I'll be taking a liberal approach to removing or combining columns.


### Skewness
I set the skewness to alert when > 1. No columns exceeded this range. Otherwise, the skewed columns would have to be handled so it turns into a more normal distribution. Here is a helpful [guide I found on kaggle](https://www.kaggle.com/code/aimack/how-to-handle-skewed-distribution) regarding this.
<br/>

### Removing similar columns
Gradient boosting and ANN are somewhat sensitive to multicollinearity so a good start is to remove columns that are similar to each other. Using the correlation matrix, we can reduce the total number of columns by simply identifying columns that are similar to each other.
![Correlation Matrix 1](/assets/2025/NFL/CorrelationMatrixOne.png)


I selected the columns to retain based on my best judgment. The bold & highlighted cells will be preserved and the rest will be ignored.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{border-color:black;border-style:solid;border-width:1px;font-family:Arial, sans-serif;font-size:14px;
  font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-8jgo{border-color:#ffffff;text-align:center;vertical-align:top}
.tg .tg-bf0q{background-color:#3166ff;border-color:#ffffff;font-weight:bold;text-align:center;vertical-align:top}
.tg .tg-o9ia{background-color:#6200c9;border-color:#ffffff;font-weight:bold;text-align:center;vertical-align:top}
</style>
<table class="tg"><thead>
  <tr>
    <th class="tg-bf0q">Similar column 1</th>
    <th class="tg-bf0q">Similar column 2</th>
    <th class="tg-bf0q">Similar column 3</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-o9ia">D_Net_Pass_Yards</td>
    <td class="tg-8jgo">D_passing_yds</td>
    <td class="tg-8jgo">D_receiving_yds</td>
  </tr>
  <tr>
    <td class="tg-8jgo">D_def_interceptions_lng</td>
    <td class="tg-o9ia">D_def_interceptions_yds</td>
    <td class="tg-8jgo"></td>
  </tr>
  <tr>
    <td class="tg-o9ia">D_passing_td</td>
    <td class="tg-8jgo">D_receiving_td</td>
    <td class="tg-8jgo"></td>
  </tr>
</tbody></table>


**Through this analysis, we are dropping the following 4 columns**: D_passing_yds, D_receiving_yds, D_def_interceptions_lng, D_receiving_td

**Creating meta-features from pairs of similar columns**
We have two sets of _X attempts + X made_ columns. I decided to replace the _X made_ columns with the percentage:

scoring_fgm / scoring_fga = scoring_fgp
scoring_xpm / scoring_xpa = scoring_xpp
punting_yds / punting_pnt = punting_avg
<br/><br/>

### Reducing sig figs
Since an EMA is applied to the majority of columns, these columns ended up containing a 4 or more sig figs. This was observed by looking at the min & max values among items with a high distinct percentage. I decided to restrict the results to 3 sig figs. 

Note that this won't be necessary if I apply a standard scaler (helpful for an ANN), however I'm applying it anyway for the second ydata_profiling run.

Observe `D_passing_yds`:
![D_passing_yds sig figs](/assets/2025/NFL/HighSigFigs.png){: width="600"}
<br /><br />



### Removing additional columns using domain knowledge
I was thinking about dropping columns _D_kick_returns_lng_ and _D_punt_returns_lng_ becaues of their similarity to _D_kick_returns_yds_ and _D_punt_returns_yds_, but decided to combine the 8 kick & punt returns into a new set of 4. Both kick & punt returns represent the same fundamental concept: the ability for a team to gain field position after receiving a ball that is kicked to them.

![Kick & Punt Returns](/assets/2025/NFL/KickPuntReturns.png){: width="700"}

Applying UMAP to reduce the following 3 columns:
 - D_kick_punt_returns_lng
 - D_kick_punt_returns_rt
 - D_kick_punt_returns_yds

We compare against a few metrics: The number of kickoffs & punts retuned for a touchdown (D_kick_punt_returns_td), the H_Won (home team won), and the D_Final (home - visitor final score) columns.

<br /><br />


![UMAP kick/punt returns colored by game winner](/assets/2025/NFL/UMAP/UmapKickPuntWon.jpg){: width="500"}
<br />
The above chart shows a boolean result in whether the home team won or lost. This shows that there is no real correlation between these four columns. I do want to verify this by considering the degree to which a team won or lost (D_Final).
<br /><br />

![UMAP kick/punt returns colored by number of kick/punt TD's](/assets/2025/NFL/UMAP/UmapKickPuntFinal.jpg){: width="500"}
<br />
The chart above shows the degree to which the home or away team won or lost. A dark green color means the home team won by a strong margin, a dark orange color means the opposite, and of course a light color means that the game was close. This confirms that the columns reduced don't show a strong correlation to the outcome of the game. There's no clear clusters or gradients.

<br /><br />

![UMAP kick/punt returns colored by number of kick/punt TD's](/assets/2025/NFL/UMAP/UmapKickPuntTd.jpg){: width="500"}
<br />
In the above diagram, we see that most of the time, a kick or punt does not result in a touchdown. There are a few noticable clusters where a gradient appears to be present.
<br /><br />
A further UMAP analysis in 2d