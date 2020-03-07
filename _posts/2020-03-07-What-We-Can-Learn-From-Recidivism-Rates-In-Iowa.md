---
layout: post
title: Tracking Recidivism in Iowa
subtitle: Its harder than you think 
bigimg:
tags: [visualization, storytelling, crime]
---

Data tracking and analysis can help policymakers evaluate the effectiveness of policy decisions and inform changes to existing proposals and aid in the shaping of new policy. It is important for those making these decisions to have the tools to properly monitor outcomes in order to create lasting and effective change. This is especially important when considering the fact that most policies are utilzing finite resources and having real effects on peoples' lives.  

One policy area that has received increased national attention in the past few election cycles has been the criminal justice system. Citizens, law makers, and advocacy groups in the United States are asking questions about why the U.S. has some of the highest rates of incarceration and recidivism in the developed world. In order to further explore this topic, I wanted to look specifically at recidivism rates and how policy initiatvies might influence these rates. I found a [dataset](https://www.kaggle.com/slonnadube/recidivism-for-offenders-released-from-prison) that tracked three year recidivism rates for prisoners in the State of Iowa.

I wanted to look at how well a model might be able to predict recidivism committed by offenders released from prison based on the initial offense class and type of the crime, the characteristics of the offender, such as race and age group, and the release type, such as ordinary end of sentence or released on parole. Additionally, I was particulary interested in looking at how one category of the dataset, 'part of target population', contirbuted to predicting recidivism. This particular category of the dataset indicated whether or not a prisoner was a participant in The Department of Corrections program created with the intention to reduce recidivism rates for prisoners who are on parole. 

![datasetgraphic](/img/datasetproject2head.PNG)

Using a random forest classifier I was only able to increase the accuracy above the majority classifier baseline by about 2%. I then looked to model tuning in order to find the best machine learning model hyperparameters for my data set. I utilized Random Search and ended up with the same accuaracy increase of 2% above the baseline. I then turned to permutation importance to look more closely at what features were having the biggest impact on my model. 

![kdeplot](/img/datasetproject2head.PNG)

```python
teamIds = [list(match['teamsData'].keys()) for match in matches_list]
teamIds_world_cup = np.unique(np.array(teamIds))
```

I then created a new dataframe with the World Cup IDs and merged it with the `players_new` dataframe in order to identify the players that played in the World Cup.

```python
teams_df = pd.DataFrame({'WorldCupTeamIDs': teamIds_world_cup})
players_teams_merged = pd.merge(players_new, teams_df, left_on='currentNationalTeamIdStr', right_on= 'WorldCupTeamIDs', how='inner')
```

The next challenge was to use each player's birth date to approximate his age at the time of the 2018 World Cup. I decided to use the starting day of the World Cup competition, June 14, 2018, in order to approximate the players' ages.

```python
player_age = (pd.Timestamp('2018-06-14') - pd.to_datetime(players_teams_merged['birthDate'])) / np.timedelta64(1, 'Y')
```

The following step was to use groupby `currentNationalTeamId` and find the average age of each team.

```python
team_age_mean = players_teams_merged.groupby(by='WorldCupTeamIDs')[['player_age']].mean()
team_age_mean.player_age = pd.to_numeric(team_age_mean.player_age)
```

To look at the relationship between average team age and number of wins, I created a new column in my dataframe with each team's total number of wins during the competition.

```python
winner_list = [str(match['winner']) for match in matches_list]
winner_df = pd.DataFrame({'WorldCupTeamIDs': winner_list})
wins = winner_df.groupby('WorldCupTeamIDs')['WorldCupTeamIDs'].count()
team_age_mean['wins'] = wins
```

Looking at the Kernel Density Estimation plot (KDE) of teams by average age, we see a steady increase in terms of density of teams with an average age of 26, 27, and 28, and then see this density take a sharp drop. The range of the plot is also fairly narrow, with average team age being no less than 24 and no greater than 31. 

![kdeplot](/img/avgworldcupplayerage.PNG)

Due to the World Cup being a knockout competition, most teams will not play the same number of matches. This situation can create [survivor bias](https://en.wikipedia.org/wiki/Survivorship_bias). Additionally, there are a greater number of teams with an average age of 27 and 28, which can also create bias in terms of the number of wins we see in this age range. 

![scatterplot](/img/avgageworldcupscatter.PNG)

In order to mitigate the effects of survivor bias, I decided to run the same code that I ran on the World Cup dataset but on the Barclays Premier League 2017/2018 competition dataset due to all teams playing the same number of matches (38). 

The KDE plot of the Premier League teams average age shows a fairly small spread, with average team age falling between about 26-28 years of age. We also see a sharper increase in terms of density of teams from 27 to 28 years of age and a sharp drop after 28 years of age. 

![engladplayerskde](/img/avgplayeragekdeengland.PNG)

There are a greater number of teams with an average player age of 27 and 28. Therefore, we would expect to see a greater number of wins for teams in this age range, which is the case, with the team with the highest number of wins having an average player age of about 28.5. The majority of teams, regardless of player age, have less than 15 wins. 

![scatterplotpremierleagueteamwins](/img/avgageenglandpointsscatter.PNG)

In order to look further into the relationship between average player age and team success, I decided to look at the total number of points earned per team during the 2017/2018 Barclays Premier League season. This number includes points earned for both wins (3) and draws (1) and this point total in turn decides the winner of the competition. 

I quickly realized that due to the complicated nesting structure of the dataset and the difficulties of linking points earned per game with team ID, I decided to use the [official data](https://www.premierleague.com/tables?co=1&se=79&ha=-1) from the Barclays Premier League website to manually create a dataframe of the total points earned per team during the 2017/2018 season. 

![scatterplotpremierleagueteampoints](/img/avgagetotalpointsengland.PNG)

We see a similar distribution as the wins scatter plot due to there being a greater number of teams with an average player age of 27 and 28. The majority of teams in this case, regardless of player age, have 50 points or less. The team with the most points at 100 is 20 points ahead of its nearest neighbor, which earned 80 points. No teams with an average player age below about 27.5 years and greater than about 28 years earned more than 50 points. 

I was surprised to find that teams with a higher average age tended to have more wins overall. It seems that players reach a performance peak around this time where peak fitness levels are coinciding with a high level of professional experience. On the other hand, I was not surprised to see a sharp drop in average team player age at around 29. Soccer is a very physically demanding sport and players are expected to maintain a high level of play throughout the duration of their club and international careers. I will be interested to see if overtime, increased use of technology in soccer and new scientific and medical advancements will lead to an increase in average team player age. In the future, I would like to perform the same code on additional league and international competitions around the world to see if my findings are replicated on a larger scale. 

From a programming perspective, I am looking forward to working with nested structures more in the future. This was my first experience working with nested structures and I found it quite challenging at times. 
