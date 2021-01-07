---
layout: post
title: Building a Product For The Restless Traveler’s Heart 
subtitle: A New Trip Planning Tool
gh-repo: https://github.com/Lambda-School-Labs/LabsPT13-Resfeber-B-DS
gh-badge: []
tags: [travel, product development, professional development]
comments: true
---

## User Stories and Project Planning 

Resfeber (RACE-fay-ber),  the restless race of the traveler’s heart before the journey begins, is the most recent project I had the opportunity to work on. My team worked with stakeholder Derek Peters to create a travel research product that allows the user to:
* Create a wish list of places to travel
* Create a profile for pinned destinations.
* View historical weather data on their destination of choice
* Create an travel Itinerary
* Explore current and historical gas and Airbnb prices/ trends and predictive data for their destination 

One of the most exciting aspects of working on this project was the opportunity to work with both front-end and back-end teams as well as a UX Designer. However, this excitement was also tinged with anxiety about how I would work with and communicate with these groups during both the planning and implementation processes. 

Our team began project planning by breaking our release into user stories. This process ensured that our product centered around the user and their needs. We discussed what kind of information our users would be seeking and in what ways they would be utilizing the product. Next, we took these questions and broke them down into tasks that would need to be completed from both a Web and Data Science perspective in order to provide these answers for our users. This planning process helped alleviate my fears of working with different teams, helping me to understand which deliverables each team would be responsible for and areas where Data Science would need to collaborate directly with the Web team. 
As a data science team, we took these user stories and started by discussing API and additional data sources we would require in order to build strong predictive pricing models for our users related to gas and Airbnb prices for their travels. A user story that I worked on, referenced in the below Trello card, was “As a user I want to be able to estimate the cost of a road trip ahead of time and choose between multiple options”.

![Trello Task](/img/Trello%20Card%20for%20Labs%20Blog.png){: .center-block :}

In response to this user story, I researched APIs and other data sources that would provide us with the necessary data to run predictive models related to gas prices and hotel prices along a user’s route. I then collaborated with my team to consider how we would store and access these data sources and what types of predictive models we would implement in order to provide the most accurate estimations of trip costs to our users. Then, I looked at the type of input these models would be receiving from the front end and made sure that we could effectively work with Web to provide this data to our users. Finally, I had to consider visuals that would also be useful to the user in relation to their question about road trip options around gas, route, and hotels. 

## Teamwork Makes the Dream Work: Collaboration and Overcoming Challenges 

The technical decisions stated above that we had to address as a team proved challenging at times. We had to consider trade-offs of utilizing one data source over another, deciding on which predictive models were the most robust, as well as how we would provide predictions based on U.S. region the traveler would be visiting. We decided to utilize the U.S. Energy Information Administration’s historical gas price data in order to predict a user’s estimated gas cost for their road trip. Using this data allowed us to run predictive models on average gas prices for the whole of the U.S. as well as look at historical gas price data for each of the major regions of the country.

A challenge that we did not foresee was the lack of complexity of the gas price data, with model inputs only including month, day, and year. In turn, our gas price predictions were exponentially increasing gas prices into the future. We recognized that this was not a robust model due to the real world realities of how supply and demand around gas prices actually operate, with both peaks and troughs present overtime. In order to improve model performance I discussed with my team the necessity of creating a new feature: for week x, I added the price of gas at week x-1 and created a new column with this information. We then explored utilizing this feature to predict gas prices further into the future in different regions of the country. Additionally, after running several types of models, we decided as a team to utilize a regression model as opposed to a neural network for predicting prices. Due to our data lacking complexity, our neural network models were overfitting and providing poor predictive performance compared to the regression models we were running. 

![Regression Model](/img/Regression%20models%20for%20gas%20prices%20for%20blog%20post.png){: .center-block :}

![Gas Price Visualization](/img/Gas%20Price%20Visualization%20For%20Labs%20Blog%20Post%20.png){: .center-block :}

## The Journey Never Truly Ends: Results and Future Implications

At this point in our project timeline, our product includes the following features for our users:
* User can login and log out utilizing Okta Auth
* User can create a trip and has the ability to search for destinations and build a trip itinerary with a starting and ending destination
* User can estimate a trip’s gas and Airbnb costs, and choose between multiple options

![Product Homepage](/img/Labs%20Website%201%20Pic.png){: .center-block :}

![Create a Trip](/img/Create%20A%20Trip%20Labs%20Project%20.png){: .center-block :}

![Trip Map](/img/Creating%20a%20Trip%20Labs%20Project%20Photo.png){: .center-block :}

For future product releases, we hope to add these additional features:
* User can add excursions, accommodations, entertainment, etc., to their itinerary.
* Within each itinerary item, the user will be able to select the activity and build a checklist, add notes, upload documents, embed links, etc.
* Users will be able to access flight information and prices as well as view/ track flights.This feature will alert the user via push notification, if any of the pinned flights are delayed.
* User will have the ability to add flight information, hotel confirmation, and information that pertains to their booked travel. This would provide the user a way to organize and track their upcoming travel,, add comments/ notes, and build upon their itinerary as needed. 

## Professional and Personal Growth 

In addition to honing my data science skills, especially around predictive modeling, I also had the opportunity to grow professionally by working with an interdisciplinary team on this project. I learned more about how to build an appropriate project timeline, work with team members of various backgrounds, and improve my project planning and communication skills. One of the biggest areas of feedback I received was how I was communicating with my teammates and sharing my feedback and ideas. I learned better ways of expressing my ideas to my team and also learning to sit back and listen before I jump in and share a particular idea or stance on what next steps we should take, etc. I have also learned the importance of project planning and really mapping out everyone’s specific roles and tasks ahead of time to help with time management and making sure every team member has clear expectations. 






