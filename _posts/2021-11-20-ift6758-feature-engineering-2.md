---
layout: post
title: Feature Engineering 2
---

## Feature Engineering 2


{% include description_df.html %}

The 2 main additional custom features are the rankings of the shooter and the goalie.


To compute the first one, we calculated the ratio of the number of goals by the sum of the number of goals and the 
number of shots for every single player of the training data.
We removed the players who took less than 15% of the average number of shots per player and per season.
Those players were given the average ranking (5). We then used every other shooter and gave them a rank (1 to 10) 
based on their success rate.


Likewise, the goalers ranking was computed using their save percentage. 
As for the 1st feature, we removed from the computation the goalies who faced less than 150 shots
so those who played on average less than 5 games. They received the average rank (5). 
The rest of the goalies got their respective rank.


In order to be able to infer these values in the test set, the new players will be given the median rank of 5 while 
the known players will get the same rank as the one they had 
in the training set while all.
- [DataFrame of Winnipeg vs Washington game on March 12, 2018](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/d70e3490cd724022b35de5a54f541ecc?assetId=76c69a6ab176417cace1287d541cd0cd&assetPath=dataframes&experiment-tab=assets)
