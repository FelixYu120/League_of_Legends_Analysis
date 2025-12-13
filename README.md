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


## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
