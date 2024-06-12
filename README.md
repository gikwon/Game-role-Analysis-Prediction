# League of Legends Role Analysis & Prediction

League of Legends, LoL for short, is a multiplayer online battle arena (MOBA) game where two teams of five players compete to destroy the other team's Nexus, a protected structure in their base..

Author: Gihyeon Kwon

## Introduction
### Introduction the Research Question
The dataset we are working with is a collection of key gameplay statistics and outcomes from a collection of competitive LoL matches collected by Oracle's Elixir. A single game is captured with 12 rows of data, first 5 consisting of the 5 players in one team and next 5 from the other team; the last 2 rows capture the overall statistic of each team. 

The 5 players in each team all have different roles, Top player, Jungle player, Middle player, Bottom player and Support player. Many have argued that the Middle position is the key role that 'carries' the team; however, others will argue that it is the Bottom position that 'carries'.

*Note: This is due to Mid and Bottom positions ability to deal high damage and make the play that can change the game. Some may disagree however..*

So in the first part of our analysis, we will be analyzing the question: ==**Which role “carries” (is the key position) in their team more often: ADCs (Bot lanes) or Mid laners?**==

### Introduction to the Data
The data we are working with contains 148,992 rows, which translates to 12,416 different matches. 

There are a total of 131 columns that describe each row but we will only be using a portion of them.
Here are some of the key columns we will be using and are good to have in mind:

Identifiers:
- `position`: Identifies the player with the role: 'top', 'jng', 'mid', 'bot', 'sup'
- `gameid`: A unique identifier for each game. As mentioned, there are 12 rows with the same `gameid`
- `side`: Identifies the team affiliation, one team as 'blue' and the other as 'red' 

Useful variables:
- `gamelength`: Shows the total game length in seconds
- `damageshare`: Shows proportion of damage done by a player relative to their team. 
- `earnedgoldshare`: Shows proportion of gold earned done by a player relative to their team. 
- `kills`: Total kills in game
- `deaths`: Total Deaths in game
- `assists`: Total Assists in game
- `firstbloodkill`: Binary variable that shows whether the player got the first kill of the game
- `firstbloodassist`: Binary variable that shows whether the player got the first assist of the game
- `dpm`: Damages done per minute
- `earned gpm`: Gold earned per minute
- `cspm`: Creep Score(CS) per minute. Every CS is gained from killing a minion or monster.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning


### Univariate Analysis

### Bivariate Analysis

### EDA

### Column Generation


## Assessment of Missingness
### NMAR Analysis

### Missingness Dependency


## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model


## Final Model


## Fairness Analysis
