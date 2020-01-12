---
layout: post
title: Unit 1 Data Storytelling Project 
subtitle: Median Team Age Vs. Wins: 2018 World Cup & Barclays Premier League 2017/2018 Season 
bigimg: /img/450px-Ronaldinho_and_Khedira.jpg
tags: [visualization, storytelling, soccer]
---

I have been passionate about soccer (or football if you aren't from the U.S.) since I started playing the game at age 5. I loved the smell of the grass on a crisp game-day morning, the rush of adrenaline as I sped towards the ball, getting to know my teammates, and the excitment of learning new skills and strategies at each game and practice session. 

As I have grown older and begun to follow professional leagues and competitions, I have become increasingly interested in how data and technology is being used to inform the game as well as allow players to stay at their peak for longer. In order to further explore this topic I decided to use my first data storytelling project to look at what if any relationship there might be between average player age per team and a team's number of wins or points. I found a [dataset](https://figshare.com/collections/Soccer_match_event_dataset/4415000/3) that had international competition as well as club competition data for 2017 and 2018.  

I began my project with the player dataset in order to obtain each player's birthdate and the ID number of the current national team they were playing for. I then created a players_new dataset with just this information. 
![players](/img/playersdf.PNG)

My next step was to look at the matches World Cup dataset in order to extract the team IDs of all 32 teams who played in the 2018 World Cup and merge this information with the players_new datatset. The challenge I ran into during this stage was that the World Cup teams dataset included some nested dictionaries. I used a list comprehension to iterate over the nested dictionary and obtain just the team IDs: 

~~~
teamIds = [list(match['teamsData'].keys()) for match in matches_list]
teamIds_world_cup = np.unique(np.array(teamIds))
~~~

I then created a new dataframe with the World Cup IDs and merged it with the players_new dataframe in order to obtain just the players that played in the World Cup:

~~~
teams_df = pd.DataFrame({'WorldCupTeamIDs': teamIds_world_cup})
players_teams_merged = pd.merge(players_new, teams_df, left_on='currentNationalTeamIdStr', right_on= 'WorldCupTeamIDs', how='inner')
~~~

The next challenge was to use the players birth date to apporixmate their age at the time of the 2018 World Cup. I decided to use the starting month of the World Cup competition, June 1, 2018, in order to approximate the players' ages: 

~~~
pd.Timestamp('2018-06-01')
player_age = (pd.Timestamp('2018-06-14') - pd.to_datetime(players_teams_merged['birthDate'])) / np.timedelta64(1, 'Y')
~~~

The following step was to use groupby 'currentNationalTeamId' and find the median age of each team:

~~~
team_age_mean = players_teams_merged.groupby(by='WorldCupTeamIDs')[['player_age']].mean()
team_age_mean.player_age = pd.to_numeric(team_age_mean.player_age)
~~~

To look at the relationship between median team age and number of wins, I created a new column in my dataframe with each of the teams total number of wins during the competition:

~~~
winner_list = [str(match['winner']) for match in matches_list]
winner_df = pd.DataFrame({'WorldCupTeamIDs': winner_list})
wins = winner_df.groupby('WorldCupTeamIDs')['WorldCupTeamIDs'].count()
team_age_mean['wins'] = wins
~~~

Looking at the kde plot of teams median age, we see a steady increase in terms of density of teams with a median age of 26, 27, and 28, and then see this density take a sharp drop. The range of the plot is also fairly narrow, with median team age being no less than 24 and not greater than 31. 

![kdeplot](/img/kdeplotworldcupteams.PNG)

Due to the World Cup being a knockout competition, most teams will not play the same number of matches. This situation can create a situation of survivor bias. Additionally, there are a greater number of teams with a median age of 27 and 28, which can also create bias in terms of the number of wins we see in this age range. 

![scatterplot](/img/scatterplotworldcupteams.PNG)

In order to mitigate the effects of survivor bias, I decieded to run the same code using the Barclays Premier League 2017/2018 competition data due to all teams playing the same number of matches during the competition year. 

![engladplayerskde](/img/premierleaguemedianplayeragekde.PNG)
