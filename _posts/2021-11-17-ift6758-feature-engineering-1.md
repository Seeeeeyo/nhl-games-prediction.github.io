---
layout: post
title: Feature Engineering 1
---

## Feature Engineering 1


### Question 1
***A histogram of shot counts (goals and no-goals separated), binned by distance.***

![per_dis](/assets/shots_per_distance_binned.png)
We can see that most of the shots are done in the range [5, 65] feet. 
This make sense since shots are taken from the inside of the offensive zone.
Both goal and shot distributions follow the same behavior. 
Both maximum are around 10 feet which is approximately the distance between the net and the zone between the 2 face-off circles.


***A histogram of shot counts (goals and no-goals separated), binned by angle.***

![per_angle](/assets/shots_per_angle_binned.png)

It is clearly observable that the histogram is symmetrical around the center 0 degree. 
This is the case for both types of events. 
Since most of the time the defensemen are protecting the zone in front of their goalies, it's gonna be much harder for the 
players of the opposing team to take a shot at an angle between [-25, 25] degrees. 
This explains the drop on the plot which is compensated from the shots taken from a higher distance.

***A 2D histogram where one axis is the distance and the other is the angle. You do not need to separate goals and no-goals.***

![per_dis_angle](/assets/shots_per_angle_and_distance_joint.png)

We chose to split the shots and goals to have some extra information on the plot even if it is not needed.
This plot is very interesting because it is a clear visualization of where the shots/goals are done on the ice.
We can draw the same observations as on the two previous histograms.
Again, we can see that most shots and goals are taken from inside the offensive zone.
There are 3 main white zones. 
The one on the far right corresponds to the defensive net. 
The one in the middle represents the area before the center line. 
Clearly there is less shots taken from there since they would lead to icing calls in a regular situation. 
But also the shots taken from this area are most likely to be blocked (since they are in the center).
The far left white zone around 0 degrees and 0 feet is coming from the assumption that all shots were aiming to the middle of the net, which is not exactly true in real life. 
However, the given data does not allow us to compute it any other way.


### Question 2
***Two more figures relating the goal rate, i.e. #goals / (#no_goals + #goals), to the distance, and goal rate to the angle of the shot***

![eff_per_dis](/assets/efficiency_per_distance_binned.png)

We can see that the goal rate decreases as the distance increases. 
It reaches its minimum at around 65 feet which is the limit of the offensive zone.
It then fluctuates a lot and reaches high rates. This is because most events recorded as goals from these distances 
are most likely happening in empty net situations.


Also, since a missed shot taken from that distance in this situations will not be counted as a shot but as a icing call.
 
In addition, the chances of a shot being taken from these distances reaching the goalie is very low, so the ratio is very high.


![eff_per_angle](/assets/efficiency_per_angle_binned.png)

As we discussed before, shots taken around 0 degrees tend to be more dangerous since there is more opening and range. 
The high ratio on the far left and far right come from the facts that: 
there are not many shots taken from there and probably very close from the goalie such as wrap-arounds.




***Create another histogram, this time of goals only, binned by distance, and separate empty net and non-empty net events.
Can you find any events that have incorrect features (e.g. wrong x/y coordinates)? If yes, prove that one event has incorrect features.***

![net](/assets/goals_per_distance_binned.png)
The distribution of the empty net goals is pretty even. This is not the case for non-empty net goals. 
Indeed, there is many of those that are suspicious cases: as we know it's very difficult for a team to score from their defensive zone. 
And if we trust this graph, we see that many of them happen from there. 
It's pretty safe to assume that many of these are coming from inconsistent data. Here is an example of such an error of the NHL data for game 2019020989:  

{% include bug.html %}
 
The 2 events are extremely close to each other in terms of time as the goal is the rebound of the first shot. 
But we also see that the x coordinates taken from the nhl api is wrong on the 
goal event as this would mean that the puck has traveled to the other side of the ice in 0 second.
This play is observable on this [video](https://www.nhl.com/video/recap-min-7-det-1/t-277350912/c-5305916) at around 66 seconds 
as well as on this [link](https://www.nhl.com/video/recap-min-7-det-1/t-277350912/c-5305916).