# League of Legends Early Game Analysis
League of Legends Early Game Analysis is a comprehensive data science project conducted at UCSD. The project encompasses various stages of analysis, from exploratory data analysis and hypothesis testing to predictive modeling and fairness analysis. The primary focus of the project is to investigate the predictive power of 15-minute economic, experience, and kill differentials (specifically Gold Difference, XP Difference, and Kill Difference) to determine whether early-game leads are reliable indicators of victory in professional esports.

Author: Felix Yu

## Introduction
### General Introduction
League of Legends (LOL) is a popular multiplayer game developed by Riot Games. It is one of the most influential and widely played games. The esports counterpart had huge successes as well. The data set we will be working with is a professional data set that’s developed by Oracle's Elixir. The file records match data from professional LOL esports gaming matches throughout 2025, including Worlds. 

This dataset captures key gameplay statistics and outcomes from a collection of LOL matches, offering a source of information for understanding player information, team information, and match outcomes.

In the realm of professional League of Legends (LoL), the concept of an "early game lead" holds significant weight and often dictates the trajectory of a match. While individual events like First Blood are flashy, the Gold Difference at 15 minutes serves as a comprehensive metric of a team's macro-performance. While this is not everything, because team fights and other major events have a more significant role in the victory or defeat of a team, early game lead is a solid metric to measure if a team is going to win. 

The central question we are interested in is: To what extent do early game economic and experience advantages (at the 15-minute mark) predict the final outcome of a professional match? We aim to use data analysis techniques to quantify the relationship between these early-game statistics, specifically golddiffat15, xpdiffat15, and killsat15, and the ultimate match result. By leveraging these indicators, we have developed a machine learning model to predict whether a team will win or lose based solely on their status at 15 minutes. This predictive model holds potential for real-time win probability estimation, helping analysts and viewers understand when a lead becomes insurmountable.

### Introduction to Dataset
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

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
