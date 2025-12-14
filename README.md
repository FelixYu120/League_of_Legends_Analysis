# League of Legends Early Game Analysis
League of Legends Early Game Analysis is a comprehensive data science project conducted at UCSD. The project encompasses various stages of analysis, from exploratory data analysis and hypothesis testing to predictive modeling and fairness analysis. The primary focus of the project is to investigate the predictive power of 15-minute economic, experience, and kill differentials (specifically Gold Difference, XP Difference, and Kill Difference) to determine whether early-game leads are reliable indicators of victory in professional esports.

Author: Felix Yu

## Introduction
### General Introduction
League of Legends (LOL) is a popular multiplayer game developed by Riot Games. It is one of the most influential and widely played games. The esports counterpart had huge successes as well. The data set we will be working with is a professional data set that’s developed by Oracle's Elixir. The file records match data from professional LOL esports gaming matches throughout 2025, including Worlds. 

This dataset captures key gameplay statistics and outcomes from a collection of LOL matches, offering a source of information for understanding player information, team information, and match outcomes.

In the realm of professional League of Legends (LoL), the concept of an "early game lead" holds significant weight and often dictates the trajectory of a match. While individual events like First Blood are flashy, the Gold Difference at 15 minutes serves as a comprehensive metric of a team's macro-performance. While this is not everything, because team fights and other major events have a more significant role in the victory or defeat of a team, early game lead is a solid metric to measure if a team is going to win. 

The central question we are interested in is: Do early-game advantages translate into match wins equally across different competitive regions, or does their impact vary by region? This question is very interesting because of the frequent debates about skill level in different regions. A common phenomenon of early game lead might be hard to overcome in one region but easier in other regions, reflecting the skill level of the teams and the overall playstyle of the region. A bystander with this information can gain further insights about how differently different regions play the game.

### Introduction to the Dataset
This dataset contains 118932 observations and 164 predictors. Since our analysis focuses on macro-level team performance, we filter these observations to isolate team-summary rows. Here is an introduction to the key columns utilized in our analysis:

- gameid: A unique identifier for each individual match played. This allows us to group data and distinguish between specific games.

- side: Denotes the map side of the team, either 'Blue' or 'Red'. In professional play, the Blue side is often statistically favored due to map geometry and pick/ban order advantages.

- result: The binary target variable indicating the match outcome. 1 indicates the team won the match, while 0 indicates a loss.

- golddiffat15: The difference in total gold between the team and their opponent at the 15-minute mark. A positive value indicates an economic lead, while a negative value indicates a deficit. This is our primary predictor for early-game advantage.

- xpdiffat15: The difference in total experience points between the team and their opponent at 15 minutes. This metric captures "level leads," which often translate into combat power spikes distinct from item gold.

- killsat15: The cumulative number of enemy champions the team has eliminated by the 15-minute mark. This measures early-game aggression and "bloodiness."

- deathsat15: The cumulative number of times the team's players have been eliminated by the 15-minute mark. High values here often correlate with mistakes or losing lanes.

- league: Denotes the professional league (e.g., LCK, LPL, LCS) where the match took place, allowing us to account for regional differences in playstyle.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The dataset is structured such that each gameid corresponds to 12 rows: 10 rows representing individual players (5 per team) and 2 rows representing the team summaries. I only kept the team rows because the analysis I am doing is a team-level analysis that analyzes the team performance at 15 minutes. So, individual player information would be unnecessary. I was also only interested in the major regions: LCK, LPL, LEC, and LTA, as they are the major pro leagues that have the highest level of gameplay. So I only selected the data from these 4 major regions and did not keep the other data. I feature engineered 'killdiffat15' as that information was not available, and I simply did it by using 'killsat15' - 'opp_killsat15' and I computed all the standardized scores as well for 'golddiffat15', 'xpdiffat15', and 'killdiffat15'. This makes it so that understanding the lead is better quantified across the 3, as the gold difference can go up to 5000, while XP and kill difference might not deviate so much. To further save time, I only kept 'gameid', 'side', 'region', 'golddiffat15', 'xpdiffat15', 'killdiffat15', 'result', 'gold_std', 'xp_std', and 'kill_std' columns because those are the only columns that actually correlate to what I am trying to find. 

Below is the head of the cleaned dataframe.
| gameid 	         | side  | region |	golddiffat15 | xpdiffat15 | killdiffat15 | result |	gold_std |	xp_std |	kill_std |
|:-----------------|:------|-------:|-------------:|-----------:|-------------:|-------:|---------:|---------|:----------|
| LOLTMNT03_183532 | Blue  | LCK 	  | 3067.0       | 2588.0     |	4.0          |	1 	  | 2.39 	   | 2.52 	 | 1.96      |
| LOLTMNT03_183532 | Red 	 | LCK 	  | -3067.0 	   | -2588.0 	  | -4.0         |	0 	  | -2.39 	 | -2.52 	 | -1.96     |
| LOLTMNT03_183538 | Blue  | LCK    |	1476.0 	     | 811.0 	    | 2.0 	       |  1 	  | 1.15     | 0.79 	 | 0.98      |
| LOLTMNT03_183538 | Red 	 | LCK 	  | -1476.0      | -811.0 	  | -2.0         |	0 	  | -1.15    | -0.79 	 | -0.98     |
| LOLTMNT03_183544 | Blue  | LCK 	  | 1993.0       | 1056.0 	  | 5.0          |	0     |	1.55     | 1.03 	 | 2.45      |

### Univariate Analysis
I performed univariate analysis on the golddiffat15 statistics in the dataset

<iframe
  src="assets/dist_gold.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the distribution of the gold difference is very normal.

I also performed univariate analysis on the xpdiffat15 statistics in the dataset

<iframe
  src="assets/dist_xp.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the distribution of the XP difference is also very normal.

I also performed univariate analysis on the killdiffat15 statistics in the dataset

<iframe
  src="assets/dist_kill.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the distribution of the Kill difference is also very normal.

### Bivariate Analysis
I performed bivariate analysis on the result versus the gold difference. 

<iframe
  src="assets/box_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The box plot showed that the average winning match had a higher gold difference at minute 15 than a lost match. This indicates how the early game economy is very important for winning a game. 

I also performed bivariate analysis for the correlation of gold difference versus xp difference at 15 minutes, and which games were won and which games were lost. 

<iframe
  src="assets/correlation_plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot showed that there is a roughly linear relationship between gold difference and xp difference. This makes sense because a gold difference and xp difference are both signals that a team is ahead, and usually when a pro team is ahead, they can capitalize on their resources and stay ahead in all resources, which includes gold and xp. We can also see that teams with more gold and XP at minute 15 have a lot higher chance of winning. Though it is not guaranteed, it proves that the two predictors are good indicators of a game's results.

### Interesting Aggregates
Here are some interesting aggregates within the data.

| gold_bucket_15 | LCK  | LEC  |	LPL |	LTA	 |	
|:---------------|:-----|:-----|:-----|:-----|
| ≤ -2000 	     | 0.10 |	0.21 |	NaN |	0.06 | 
| -2000 to -1000 | 0.26 |	0.27 |	NaN |	0.42 |
| -1000 to 0     | 0.44 |	0.46 |	NaN |	0.25 |
| 0 to 1000 	   | 0.56 |	0.54 |	NaN |	0.75 |
| 1000 to 2000 	 | 0.74 |	0.73 |	NaN |	0.58 |
| ≥ 2000 	       | 0.90 |	0.79 |	NaN |	0.94 |

I did a pivot table based on how ahead the team is and if they won the match or not. These are represented in percentages so 90 percent chance that a team in LCK with a greater than 2000 gold lead can win the game. And interestingly, it was all NAN for LPL teams. 

## Assessment of Missingness
### NMAR Analysis
I believe that the ban columns are NMAR because the nature of the missingness depends on the value itself. When a row is missing a ban1 or ban2 it implies that there was no ban on any champion at all. Because this was not dependent on any other values, this constitutes an NMAR. 

### Missingness Dependency
In this part, we computed whether golddiffat15's missingness is dependent on the region. I did this because of the table above, where LPL had NaN for gold. I used a permutation test to see if the missingness in golddiffat15 was just random or it came from another distribution. I used a 0.05 significance level cutoff and used TVD for the test statistic. 
Null Hypothesis: The distribution of league when golddiffat15 is missing is the same as the distribution of league when golddiffat15 is not missing.
Alternative Hypothesis: The distribution of league when golddiffat15 is missing is not the same as the distribution of league when golddiffat15 is not missing.
Below is the graph of the permutation test of golddiffat15

<iframe
  src="assets/tvd_plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This shows that it is significant, we reject the null hypothesis, and that golddiffat15 is very dependent on the region. And with further data analysis, I found out that all LPL's statistics for anything related to the actual game content are missing. This might be due to a privacy issue in China, where they don't want to release their information. This proves that it is MAR, meaning that the missingness is because of another column: region. I then looked at other regions that are not lpl and found out that they had no missingness at all.

I then proceeded to a non-important predictor: damagetotower. This is not important to my analysis because I only care about early game statistics, and this is very unrelated. I used a 0.05 significance level and used the TVD again for the test statistic.
Null Hypothesis: The distribution of league when damagetotower is missing is the same as the distribution of league when damagetotower is not missing.
Alternative Hypothesis: The distribution of league when damagetotower is missing is not the same as the distribution of league when damagetotower is not missing.
Below is the graph

<iframe
  src="assets/tvd_plot2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Similarly, it showcased that it is very significant, and we reject the null hypothesis. In a similar fashion to the other missingness analysis, it is only the Chinese LPL that did not have any data. All other regions had no missing data in this as well. 

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
