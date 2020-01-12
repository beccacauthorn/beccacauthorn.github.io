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

teamIds = [list(match['teamsData'].keys()) for match in matches_list]

teamIds_world_cup = np.unique(np.array(teamIds))

I then created a new dataframe with the World Cup IDs and merged it with the players_new dataframe: 

teams_df = pd.DataFrame({'WorldCupTeamIDs': teamIds_world_cup})

players_teams_merged = pd.merge(players_new, teams_df, left_on='currentNationalTeamIdStr', right_on= 'WorldCupTeamIDs', how='inner')
