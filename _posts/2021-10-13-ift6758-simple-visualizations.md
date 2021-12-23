---
layout: post
title: Simple Visualizations
---

## Simple Visualizations

### Question 1
*Produce a histogram or barplot of shot types over all teams in a season of your choosing. Overlay the number of goals overtop the number of shots. What appears to be the most dangerous type of shot? The most common type of shot?*

The most commom type of shot during the season 2016-2017 is the wrist shot with about 4100 goals and 42k saved shots (so about 46k shots in total). 


The most dangerous type of shot for this season in terms of success rate is the deflected one with 211 goals over 884 saved shots. 

However, the shot type which resulted in the most number of goals is again the wrist shot with 4096 goals. 


{% include shots_per_type.html %}


### Question 2
*What is the relationship between the distance a shot was taken and the chance it was a goal? Produce a figure for each season between 2018-19 to 2020-21 to answer this.  Has there been much change over the past three seasons?*

We decided to bin the distance in order to switch from continuous to discrete data.
There 90 bins in total so the distance interval of each is about 2 feet.

The plots below show the successs rate for each bin. For a particular bin, the success rate corresponds to the number of goals from this distance range divided by the total number of shots from there.

We decided to not show the bins but only the line fitting the y value of each bin for more readability.

<!--{% include shot_distribution_season2018.html %}-->
<!--{% include shot_distribution_season2019.html %}-->
<!--{% include shot_distribution_season2020.html %}-->
{% include shot_distribution_line_season2018.html %}
{% include shot_distribution_line_season2019.html %}
{% include shot_distribution_line_season2020.html %}

We do observe quite a similar behavior for season 2018-2019 and 2019-2020. But there is a few differences such as the higher rate at maximum distances (around 185 feet) for season 2019-2020.

The success rate at 2 feet during the season 2020-2021 is lower than the 2 previous seasons (0.63 vs 0.77 and 0.8). Besides that, the last graph is very similar to the other 2. 

As expected, there are high success rates for small distances. Since there is not many shots from far away, and that they're only recorded when they are on target, the success rate at high distances make sense as well. Most or all the goal at such distances are done with an empty net. 


### Question 3
*Produce a figure that shows the goal percentage (#goals / #shots) and discuss e.g. what might be the most dangerous types of shots?*


As we can observe on the heatmap below:
* The deflected shots and Tip-in at [0-10] feet away from the net have the largest success rate (sr) (0.3) after the wrap-around (sr=0.5) which is an outlier. It's thus the most dangerous type of shots. We'll comeback to the wrap around case later. 
* The wrap around shots have a very small/null success rate in general. The sr of the wrap around shots at around 30 feet could appear suspicious since it's quite far for such a shot but we verified the data to make sure that it really happened. The high ratio comes from the fact that only 2 shots of this type were done at this distance.
* Between 70 and 180 feet, the most sucessful type of shot is the Tip-In.
* A further analysis could be to investigate whether or not the goals were scored in an empty net. Thus would bring us a 3rd feature and it would allow us to be more precise in terms of shot analysis.
* At the maximum distance (around 180 feet), the wrist shot is the most successful one because it's the most precise one in general and especially from such a distance.


{% include shots_per_type_per_distance_heatmap.html %}


