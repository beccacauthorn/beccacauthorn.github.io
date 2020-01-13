---
layout: post
title: Unit 1 Data Storytelling Project 
subtitle: How player age affects success 
bigimg: /img/450px-Ronaldinho_and_Khedira.jpg
tags: [visualization, storytelling, soccer]
---

I have been passionate about soccer (or football if you aren't from the U.S.) since I started playing the game at age 5. I loved the smell of the grass on a crisp game-day morning, the rush of adrenaline as I sped towards the ball, getting to know my teammates, and the excitement of learning new skills and strategies at each game and practice session. 

As I have grown older and begun to follow professional leagues and competitions, I have become increasingly interested in how data and technology are being used to inform the game as well as allow players to stay at their peak for longer. In order to further explore this topic, I decided to use my first data storytelling project to look at what, if any, relationship there might be between average player age per team, and a team's number of wins or points. I found a [dataset](https://figshare.com/collections/Soccer_match_event_dataset/4415000/3) that had international competition as well as club competition data for 2017 and 2018.  

I began my project with the player dataset in order to obtain each player's birthdate and the ID number of the current national team they were playing for. I then created a players_new dataset with just this information. 
![players](/img/playersdf.PNG)

My next step was to look at the matches World Cup dataset in order to extract the team IDs of all 32 teams who played in the 2018 World Cup and merge this information with the players_new datatset. The challenge I ran into during this stage was that the World Cup teams dataset included some nested dictionaries. I used a list comprehension to iterate over the nested dictionary and obtain just the team IDs: 

```python
teamIds = [list(match['teamsData'].keys()) for match in matches_list]
teamIds_world_cup = np.unique(np.array(teamIds))
```

I then created a new dataframe with the World Cup IDs and merged it with the players_new dataframe in order to obtain just the players that played in the World Cup:

```python
teams_df = pd.DataFrame({'WorldCupTeamIDs': teamIds_world_cup})
players_teams_merged = pd.merge(players_new, teams_df, left_on='currentNationalTeamIdStr', right_on= 'WorldCupTeamIDs', how='inner')
```

The next challenge was to use the players birth date to approximate their age at the time of the 2018 World Cup. I decided to use the starting month of the World Cup competition, June 1, 2018, in order to approximate the players ages.

```python
pd.Timestamp('2018-06-01')
player_age = (pd.Timestamp('2018-06-14') - pd.to_datetime(players_teams_merged['birthDate'])) / np.timedelta64(1, 'Y')
```

The following step was to use groupby 'currentNationalTeamId' and find the median age of each team.

```python
team_age_mean = players_teams_merged.groupby(by='WorldCupTeamIDs')[['player_age']].mean()
team_age_mean.player_age = pd.to_numeric(team_age_mean.player_age)
```

To look at the relationship between median team age and number of wins, I created a new column in my dataframe with each of the teams total number of wins during the competition.

```python
winner_list = [str(match['winner']) for match in matches_list]
winner_df = pd.DataFrame({'WorldCupTeamIDs': winner_list})
wins = winner_df.groupby('WorldCupTeamIDs')['WorldCupTeamIDs'].count()
team_age_mean['wins'] = wins
```

Looking at the Kernel Density Estimation plot(kde) of teams median age, we see a steady increase in terms of density of teams with a median age of 26, 27, and 28, and then see this density take a sharp drop. The range of the plot is also fairly narrow, with median team age being no less than 24 and not greater than 31. 

![kdeplot](/img/kdeplotworldcupteams.PNG)

Due to the World Cup being a knockout competition, most teams will not play the same number of matches. This situation can create survivor bias. Additionally, there are a greater number of teams with a median age of 27 and 28, which can also create bias in terms of the number of wins we see in this age range. 

![scatterplot](/img/scatterplotworldcupteams.PNG)

In order to mitigate the effects of survivor bias, I decieded to run the same code on this dataset but using the Barclays Premier League 2017/2018 competition data due to all teams playing the same number of matches (38). 

The kde plot of the Premier League teams median age shows a fairly small spread, with median team age falling between about 26-28 years. We also see a sharper increase in terms of density of teams from 27 to 28 years and a sharp drop in median age after 28 years. 

![engladplayerskde](/img/premierleaguemedianplayeragekde.PNG)

There are a greater number of teams with a median age of 27 and 28. Therefore, we would expect to see a greater number of wins for teams in this age range, which is the case, with the team with the most number of wins having an average age of about 28.5. The majority of teams, regardless of age, have less than 15 wins. 

![scatterplotpremierleagueteamwins](/img/premierleaguenumberofwinsscatter.PNG)

In order to look further into the relationship between average player age and team success, I decided to look at the total number of points earned per team during the 2017/2018 Barclays Premier League season. This number includes points earned for both wins (3) and draws (1) and this point total in turn decides the winner of the competition. 

I quickly realized that due to the complicated nesting structure of the dataset and the difficulties of linking points earned per game with team ID, I decided to use the [official data](https://www.premierleague.com/tables?co=1&se=79&ha=-1) from the Barclays Premier League website to manually create a dataframe of the total points earned per team during the 2017/2018 season. 

![scatterplotpremierleagueteampoints](/img/premierleagueteamstotalpointsscatter.PNG)

We see a similar distribution as the wins scatter plot due to there being a greater number of teams with a median age of 27 and 28. The majority of teams in this case, regardless of age, have 50 points or less. The team with the most points at 100, is 20 points ahead of its nearest neighbor who earned 80 points. No teams with an average age below about 27.5 years and greater than about 28 years earned more than 50 points. 

I was surprised to find that teams with a higher average age tended to have more wins overall. It seems that players reach a performance peak around this time where peak fitness levels are coinciding with a high level of professional experience. On the other hand, I was not surprised to see a sharp drop in average team age at around 29. Soccer is a very physically demanding sport and players are expected to maintain a high level of play throughout the duration of their club and international careers. I will be interested to see if overtime, with increased use of technology in soccer and new scientific and medical advancements, leads to an increase in average team age. In the future, I would like to perform the same code on additional league and international competitions around the world to see if my findings are replicated on a larger scale. 

From a programming perspective, I am looking forward to working with nested structures more in the future. This was my first experience working with nested structures and I found it quite challenging at times. 


--------------------------------
Photo source: [Wikipedia](https://en.wikipedia.org/wiki/Association_football)
