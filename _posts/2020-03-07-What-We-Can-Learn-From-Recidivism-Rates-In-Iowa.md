---
layout: post
title: Tracking Recidivism in Iowa
subtitle: Its harder than you think 
bigimg:
tags: [visualization, storytelling, policy]
---

Data tracking and analysis can help policymakers evaluate the effectiveness of policy decisions and inform changes to existing proposals as well as aid in the shaping of new policy. It is important for those making these decisions to have the tools to properly monitor outcomes in order to create lasting and effective change. This is especially important when considering the fact that most policies are utilzing finite resources and having real effects on peoples' lives.  

One policy area that has received increased national attention in the past few election cycles has been the criminal justice system. Citizens, lawmakers, and advocacy groups in the United States are asking questions about why the U.S. has some of the highest rates of incarceration and recidivism in the [world](https://www.businessinsider.com/why-norways-prison-system-is-so-successful-2014-12). In order to further explore this topic, I wanted to look specifically at recidivism rates and how policy might influence these rates. I found a [dataset](https://www.kaggle.com/slonnadube/recidivism-for-offenders-released-from-prison) that tracked three year recidivism rates for prisoners in the state of Iowa.

I wanted to look at how well a model might be able to predict recidivism committed by offenders released from prison based on the initial offense class and type of the crime, the characteristics of the offender, such as race and age group, and the release type, such as ordinary end of sentence or released on parole. Additionally, I was particulary interested in looking at how one feature of the dataset, 'part of target population', contirbuted to predicting recidivism. This particular feature of the dataset indicated whether or not a prisoner was a participant in The Department of Corrections program created with the intention to reduce recidivism rates for prisoners who are on parole. 

![datasetgraphic](/img/datasetproject2head.PNG)

Using a random forest classifier I was only able to increase the accuracy above the majority classifier baseline by about 2%. I then looked to model tuning in order to find the best machine learning model hyperparameters for my data set. I utilized Random Search and ended up with the same accuaracy increase of 3% above the baseline. I then turned to permutation importance to look more closely at what features were having the biggest impact on my model. According to the permutation importance chart below, a prisoners' age is the most important feature in the model. 

![permutationimportancechart](/img/permutationimportanceunit2project.PNG)

In addition to permutation importance, I created a partial dependence plot in order to look more closely at the relationship between prisoners' age at the time of release and the target feature (1= Under the age of 25, 2= aged 25-34, 3= aged 35-44, 4= aged 45-54, and 5= aged 55 and Older). 

![PDPAge](/img/pdpplotunit2project.PNG)

Here we see that increasing age correlates to a lower chance of recidivism. 

In order to gain more insight into not only the errors being made by my model but also the types of errors that were being made, I created a confusion matrix (1 means 'yes recidivism' and 0 means 'no recidivism'). 

![ConfusionMatrix](/img/confusionmatrixunit2project.PNG)

The confusion matrix shows us that accuracy is around 66% and that precision, or positive predictive value, is 55%. 
