---
layout: post
title: Baseline Models
---

## Baseline Models


### Question 1
***Evaluate the accuracy (i.e. correctly predicted / total) of your model on the validation set. What do you notice? Look at the predictions and discuss your findings. What could be a potential issue? Include these discussions in your blog post.***

The accuracy of the logistic regression model on the validation set is 0.91.
At the first look, it is a pretty good score, especially since we are only using the distance feature. 
However, while inspecting the predictions, we observe the following: 

Number of goal predictions: 0 ----- Number of shots predictions: 93326. 


This means that the model predicts all the sample as shots (non goals). 
The potential issue is that the model would never increase its true positive rate (true goals) because the data is very unbalanced. 
Indeed, there is only 29176 goals against 311086 shots (x10).
To face this problem, we could re-balance the data by applying an oversampling or undersampling method. 


### Question 2
***Receiver Operating Characteristic (ROC) curves and the AUC metric of the ROC curve. Include a random classifier baseline, i.e. each shot has a 50% chance of being a goal.***

![log_reg_roc](/assets/log_reg_roc.png)

{% include roc_auc_log_reg.html %}

We can see on these roc curves that the model trained on the angle only is not performant as its 
AUC is almost equal to the random baseline's AUC. 
The best 2 models are the ones trained on distance and distance + angle reaching a AUC just below 0.7.
This means that the contribution of the angle feature on the logistic regression model is very low/null. 

***The goal rate (#goals / (#no_goals + #goals)) as a function of the shot probability model percentile, i.e. if a value is the 70th percentile, it is above 70% of the data.***

{% include goal_rate_log_reg.html %}

It's observable that the random baseline is more or less uniform around 8%. 
This is because there is 8 % of goals in the data.
The models which have at least the distance as training feature can definitely output higher goal probabilities as 
it was previously demonstrated in multiple previous discussions.
Again, we see that the angle feature is not contributing much to the probability of the logistic regression.


***The cumulative proportion of goals (not shots) as a function of the shot probability model percentile.***

{% include cumulative_perc_log_reg.html %}

This plot is very similar to the roc plot. 



***The reliability diagram (calibration curve).***

![calibration](/assets/calibration_log_reg.png)

The maximum probability that the logistic regression models output is around 0.2 
and this is why their plots do not go above this value. 
All of these are quite perfectly calibrated and this make sense since logistic regression is one of the algorithms 
which calibrates its predicted probabilities ([source](https://machinelearningmastery.com/calibrated-classification-model-in-scikit-learn/)).
We can also observe that the probabilities from the model trained on the angle only are all around 0.1. 

*Links to the experiments in comet:*
- [Log Reg trained on distance](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/62cd9f9c20aa46b0828c9ddd04250dcc?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=wall)
- [Log Reg trained on angle](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/a269bb302c6542bd8c2176ffacfb2545?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=wall)
- [Log Reg trained on distance and angle](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/a2fb655b76aa4b8f90e9e8092c2d96b5?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=wall)
