# League of Legends Role Analysis & Prediction

League of Legends, LoL for short, is a multiplayer online battle arena (MOBA) game where two teams of five players compete to destroy the other team's Nexus, a protected structure in their base..

Author: Gihyeon Kwon

## Introduction
### Introduction the Research Question
The dataset we are working with is a collection of key gameplay statistics and outcomes from a collection of competitive LoL matches collected by Oracle's Elixir. A single game is captured with 12 rows of data, first 5 consisting of the 5 players in one team and next 5 from the other team; the last 2 rows capture the overall statistic of each team. 

The 5 players in each team all have different roles, Top player, Jungle player, Middle player, Bottom player and Support player. Many have argued that the Middle position is the key role that 'carries' the team; however, others will argue that it is the Bottom position that 'carries'.

\**Note: This is due to Mid and Bottom positions ability to deal high damage and make the play that can change the game. Some may disagree however..*

So in the first part of our analysis, we will be analyzing the question: **Which role “carries” (is the key position) in their team more often: ADCs (Bot lanes) or Mid laners?**

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
The column `datacompleteness` shows whether the data is complete, partially complete, or need to be ignored. I have only kept rows that are complete. Also since I won't being needing the 2 rows of the team statistics for each game, I have removed those 2 rows for every game. That left me with 105530 rows or 10553 games.

I have also changed the `result` column into binary type, which shows 1 if the team won the game and 0 if lost.

Then I have created some new columns that may be useful for the analysis:

- `gamelength_minutes`: This column represents the length of the game in minutes, calculated by dividing the `gamelength` column by 60.

- `kills_per_minute`: This column represents the number of kills per minute, calculated by dividing the `kills` column by `gamelength_minutes`.

- `assists_per_minute`: This column represents the number of assists per minute, calculated by dividing the `assists` column by `gamelength_minutes`.

- `deaths_per_minute`: This column represents the number of deaths per minute, calculated by dividing the `deaths` column by `gamelength_minutes`.

- `kd_ratio`: This column represents the kill-to-death ratio, calculated by dividing the `kills` column by the sum of the `deaths` column plus one.

- `kda_ratio`: This column represents the kill-death-assist ratio, calculated by dividing the sum of the `kills` and `assists` columns by the sum of the `deaths` column plus one.

- `kill_participation`: This column represents the kill participation rate, calculated by dividing the sum of the `kills` and `assists` columns by the sum of the `teamkills` column plus one.

- `team_kda`: This column represents the average KDA ratio of the team, calculated by grouping by `gameid` and `side`, and then taking the mean of the `kda_ratio` column within each group.

- `team_kd`: This column represents the average KD ratio of the team, calculated by grouping by `gameid` and `side`, and then taking the mean of the `kd_ratio` column within each group.

- `kda_normal`: This column represents the normalized KDA ratio, calculated by dividing the `kda_ratio` column by the sum of the `team_kda` column plus one.

- `kd_normal`: This column represents the normalized KD ratio, calculated by dividing the `kd_ratio` column by the sum of the `team_kda` column plus one.

**The reason for creating these columns was to normalize the key metrics for each game. Since we will be comparing each metric across games that are different in game length. Ex. 10 kills in a 30 minute game isn't same as 10 kills in a 15 minute game**

Below is the first 5 rows of cleaned data.

| gameid                | datacompleteness   |   game |   patch |   participantid | side   | position   | playername   | playerid                                  | teamname                 | teamid                                  | champion   |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   firstbloodkill |   firstbloodassist |   firstbloodvictim |   team kpm |   ckpm |   firstdragon |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders |   firstherald |   heralds |   opp_heralds |   void_grubs |   opp_void_grubs |   firstbaron |   barons |   opp_barons |   firsttower |   towers |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   damagetochampions |     dpm |   damageshare |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   earned gpm |   earnedgoldshare |   goldspent |   gspd |   gpr |   total cs |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |   cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |   gamelength_minutes |   kills_per_minute |   assists_per_minute |   deaths_per_minute |   kd_ratio |   kda_ratio |   kill_participation |   team_kda |   team_kd |   kda_normal |   kd_normal |
|:----------------------|:-------------------|-------:|--------:|----------------:|:-------|:-----------|:-------------|:------------------------------------------|:-------------------------|:----------------------------------------|:-----------|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-----------------:|-------------------:|-------------------:|-----------:|-------:|--------------:|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|-------------------------:|---------:|-------------:|--------------:|----------:|--------------:|-------------:|-----------------:|-------------:|---------:|-------------:|-------------:|---------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|-------------:|-----------------:|--------------------:|--------:|--------------:|-----------------------:|---------------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|-------------:|------------------:|------------:|-------:|------:|-----------:|--------------:|---------------:|------------------------:|--------------------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|---------------------:|-------------------:|---------------------:|--------------------:|-----------:|------------:|---------------------:|-----------:|----------:|-------------:|------------:|
| ESPORTSTMNT01_2690210 | complete           |      1 |   12.01 |               1 | Blue   | top        | Soboro       | oe:player:38e0af7278d6769d0c81d7c4b47ac1e | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Renekton   |         1713 | False    |       2 |        3 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               15768 | 552.294 |     0.278784  |               1072.4   |                    777.793 |             8 | 0.2802 |             6 | 0.2102 |                    5 |            26 | 0.9107 |       10934 |         7164 |      250.928 |          0.253859 |       10275 |    nan |   nan |        231 |           220 |             11 |                     nan |                       nan | 8.0911 |       3228 |     4909 |       89 |           3176 |         4953 |           81 |             52 |          -44 |            8 |           0 |             0 |            0 |               0 |                 0 |                0 |       5025 |     7560 |      135 |           4634 |         7215 |          121 |            391 |          345 |           14 |           0 |             1 |            0 |               0 |                 1 |                0 |                28.55 |          0.0700525 |            0.0700525 |           0.105079  |   0.5      |     1       |                  0.4 |    1.19333 |  0.413333 |     0.455927 |   0.227964  |
| ESPORTSTMNT01_2690210 | complete           |      1 |   12.01 |               2 | Blue   | jng        | Raptor       | oe:player:637ed20b1e41be1c51bd1a4cb211357 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Xin Zhao   |         1713 | False    |       2 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                1 |               11765 | 412.084 |     0.208009  |                944.273 |                    650.158 |             6 | 0.2102 |            18 | 0.6305 |                    6 |            48 | 1.6813 |        9138 |         5368 |      188.021 |          0.19022  |        8750 |    nan |   nan |        148 |            33 |            115 |                     nan |                       nan | 5.1839 |       3429 |     3484 |       58 |           2944 |         3052 |           63 |            485 |          432 |           -5 |           1 |             2 |            0 |               0 |                 0 |                1 |       5366 |     5320 |       89 |           4825 |         5595 |          100 |            541 |         -275 |          -11 |           2 |             3 |            2 |               0 |                 5 |                1 |                28.55 |          0.0700525 |            0.210158  |           0.175131  |   0.333333 |     1.33333 |                  0.8 |    1.19333 |  0.413333 |     0.607903 |   0.151976  |
| ESPORTSTMNT01_2690210 | complete           |      1 |   12.01 |               3 | Blue   | mid        | Feisty       | oe:player:d1ae0e2f9f3ac1e0e0cdcb86504ca77 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | LeBlanc    |         1713 | False    |       2 |        2 |         3 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |                0 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               14258 | 499.405 |     0.252086  |                581.646 |                    227.776 |            19 | 0.6655 |             7 | 0.2452 |                    7 |            29 | 1.0158 |        9715 |         5945 |      208.231 |          0.210665 |        8725 |    nan |   nan |        193 |           177 |             16 |                     nan |                       nan | 6.7601 |       3283 |     4556 |       81 |           3121 |         4485 |           81 |            162 |           71 |            0 |           0 |             1 |            0 |               0 |                 0 |                1 |       5118 |     6942 |      120 |           5593 |         6789 |          119 |           -475 |          153 |            1 |           0 |             3 |            0 |               3 |                 3 |                2 |                28.55 |          0.0700525 |            0.105079  |           0.0700525 |   0.666667 |     1.66667 |                  0.5 |    1.19333 |  0.413333 |     0.759878 |   0.303951  |
| ESPORTSTMNT01_2690210 | complete           |      1 |   12.01 |               4 | Blue   | bot        | Gamin        | oe:player:998b3e49b01ecc41eacc392477a98cf | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Samira     |         1713 | False    |       2 |        4 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                0 |                  1 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |               11106 | 389.002 |     0.196358  |                463.853 |                    218.879 |            12 | 0.4203 |             6 | 0.2102 |                    4 |            25 | 0.8757 |       10605 |         6835 |      239.405 |          0.242201 |       10425 |    nan |   nan |        226 |           208 |             18 |                     nan |                       nan | 7.9159 |       3600 |     3103 |       78 |           3304 |         2838 |           90 |            296 |          265 |          -12 |           1 |             1 |            0 |               0 |                 0 |                0 |       5461 |     4591 |      115 |           6254 |         5934 |          149 |           -793 |        -1343 |          -34 |           2 |             1 |            2 |               3 |                 3 |                0 |                28.55 |          0.0700525 |            0.0700525 |           0.140105  |   0.4      |     0.8     |                  0.4 |    1.19333 |  0.413333 |     0.364742 |   0.182371  |
| ESPORTSTMNT01_2690210 | complete           |      1 |   12.01 |               5 | Blue   | sup        | Loopy        | oe:player:e9741b3a238723ea6380ef2113fae63 | Fredit BRION Challengers | oe:team:68911b3329146587617ab2973106e23 | Leona      |         1713 | False    |       1 |        5 |         6 |           9 |           19 |             0 |             0 |             0 |            0 |            1 |                1 |                  0 |                  0 |     0.3152 | 0.9807 |           nan |       nan |           nan |               nan |                   nan |         nan |         nan |      nan |      nan |         nan |        nan |                      nan |      nan |          nan |           nan |       nan |           nan |          nan |              nan |          nan |        0 |            0 |          nan |      nan |          nan |             nan |                  nan |            nan |                nan |            0 |                0 |                3663 | 128.301 |     0.0647631 |                475.026 |                    490.123 |            29 | 1.0158 |            14 | 0.4904 |                   11 |            69 | 2.4168 |        6678 |         2908 |      101.856 |          0.103054 |        6395 |    nan |   nan |         42 |            42 |              0 |                     nan |                       nan | 1.4711 |       2678 |     2161 |       16 |           2150 |         2748 |           15 |            528 |         -587 |            1 |           1 |             1 |            0 |               0 |                 0 |                1 |       3836 |     3588 |       28 |           3393 |         4085 |           21 |            443 |         -497 |            7 |           1 |             2 |            2 |               0 |                 6 |                2 |                28.55 |          0.0350263 |            0.210158  |           0.175131  |   0.166667 |     1.16667 |                  0.7 |    1.19333 |  0.413333 |     0.531915 |   0.0759878 |

\**Note: The columns will look messy here but I have kept many of the columns to use later in my analysis*

### Univariate Analysis

First I performed univariate analysis on the normalized KDA Ratio between mid position and bottom positions:

<iframe
  src="assets/Boxplot_KDA.html"
  width="1000"
  height="800"
  frameborder="0"
></iframe>
The box plot shows the distribution of each position's normalzied KDA and we can see that the min, lower quantile median, and the upper quantile for bottom position is slighter higher than that of midd;e positions.

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
