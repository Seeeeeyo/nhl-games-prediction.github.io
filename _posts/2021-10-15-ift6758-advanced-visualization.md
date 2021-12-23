---
layout: post
title: Advanced Visualizations
---

## Advanced Visualizations


### Question 1
*Your plot must allow users to select any season between 2016-17 and 2020-2021, as well as any team during the selected season.*


Please be patient with the visualization, the server is not very fast. Also, make sure to select a season first and then a team.


<iframe id="igraph" class='frame' scrolling="no" style="border:none;" seamless="seamless" src="https://dashboard6758.herokuapp.com" height="1000" width="350%"></iframe>



### Question 2
*Discuss (in a few sentences) what you can interpret from these plots.*


These plots allow us to make some observations about where specific teams tend to shot compared to the league. 
This gives us insights about the strategy of the different team. 
A team could study the plots of its opponent before a game in order to analyse and prepare the best defense strategy. 

Since we know from the previous graph that the success rate of shots is higher at small distances (close from the net), 
we can compare the visualizations for different team and thus explain the success of a given team with the amount of shots that they take from short distances.


We can also see which teams have offensive defense players by looking at the heatmap zones near the blue line (locations of the defensemen). If these zones are red-ish (values above 0), then we can conclude that they are more offensive than the average.


### Question 3
*Consider the Colorado Avalanche; take a look at their shot map during the 2016-17 season. Discuss what you could say about the team during this season. Now look at the shot map for the Colorado Avalanche for the 2020-21 season, and discuss what you could conclude from these differences. Does this make sense?*


While they seem to be pretty good in the pain zone in 2016-2017, they were still behind the league average for all the other zones (shot number wise). 
We can see that they had good shot productivity coming from their forwards players but they lacked a lot of shot diversity, especially coming from the defensemen position.
Therefore, we expect this team to be in the end of the ranking.



<!--![hm_2016_col](/assets/hm_2016_col.png) -->



**Colorado Avalanche 2016-17:**

{% include shotmap_CA_2016.html %}


 We checked the stats and they were indeed at the bottom of the league with only 22 wins over 82 games.
 
 
![ranking](/assets/ranking.png) 

We can see that their 2020-21 team took a lot of shots on almost every area of the offensive zone. There is a very big positive area in front of the goal, meaning that they took more shots than the league average. Thus, we can suppose that they scored more and won more games. Since they shoot from everywhere, that made them very hard to predict offensive-wise. Again, the stats confirm our hypothesis since they won 39 out of 56 games during this particular season.
**Colorado Avalanche 2020-21:**
{% include shot_distribution_CA_2020.html %}


### Question 4
*Consider the Buffalo Sabres, which have been a team that has struggled over recent years, and compare them to the Tampa Bay Lightning, a team which has won the Stanley for the past two years in a row. Look at the shot maps for these two teams from the 2018-19, 2019-20, and 2020-21 seasons. Discuss what observations you can make. Is there anything that could explain the Lightning’s success, or the Sabres’ struggles? How complete of a picture do you think this paints?*


**Buffalo Sabres 2018-19:**
{% include shot_distribution_BS_2018.html %}


**Buffalo Sabres 2019-20:**
{% include shot_distribution_BS_2019.html %}


**Buffalo Sabres 2020-21:**
{% include shot_distribution_BS_2020.html %}


We can observe that the Sabres only shoot from a few different areas of the offensive zone. This made them very vulnerable in terms of shoot prediction.
They have some zones in which they shoot more than the league average but most of them are far away from the goal so they have less chance to score. 
They are pretty okay around the net but very inoffensive on other zones such as in the 2 circles. 


 



**Tampa Bay Lightning 2018-19:**
{% include shot_distribution_TBL_2018.html %}


**Tampa Bay Lightning 2019-20:**
{% include shot_distribution_TBL_2019.html %}


**Tampa Bay Lightning 2020-21:**
{% include shot_distribution_TBL_2020.html %}



Whereas the Tempa Bay shoot more than the league average in many areas, especially at the corners of the goal and right in front of it. They also have positive values in the circles, which are known to be dangerous in offense. We can also see that the Lightning's shots are very well distributed.
We thought that the last visualization (Tampa Bay Lightning 2020-21) was weird since they are quite at the same level (in terms of shots) than the league, especially since they won the Stanley Cup that year. They had 1692 shots while the league average was 1678.
But we checked the nhl stats and found out that they were right on the league average for shot numbers.  Actually, they were in a very competitive and special division due to covid restrictions. 

