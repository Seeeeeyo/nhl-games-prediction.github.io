---
layout: post
title: Advanced Models
---

## Advanced Models

### Question 1
***XGBoost classifier using the same dataset using only the distance and angle features. 
Briefly discuss your training/validation setup, and compare the results to the Logistic Regression baseline.***

![roc](/assets/log_reg_rocQ51.png)
{% include xgboost_basic.html %}
{% include cumulative_perc_log_regQ51.html %}
{% include goal_rate_log_regQ51.html %}
![reliability](/assets/reliability_plotQ51.png)


*Links to the experiments in comet:*
- [XGBoost trained on distance](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/2248b4334a734b3ba44ab23f72c819f7?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)
- [XGBoost trained on angle](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/a59023b84bdf4c96ba1fe00ec393f359?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)
- [XGBoost trained on distance and angle](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/797a26ac2cf24195a693a78b1748c5ce?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)


We simply used the *train_test_split* function from sklearn with a validation percentage of 20. 
In this basic model, we have not applied any extra step such as normalization or encoding. 


Differences between these models and the logistic regression baseline: 
- XGBoost is already performant with only the angle feature (not the case for the baseline).
- The improvements coming from adding the angle feature to the distance one are greater in the XGBoost model than in the baseline.
- The best basic XGBoost's performance is higher than the baseline but not that much (difference of AUC= ~0.01).


### Question 2
***XGBoost classifier using all of the features you created in Part 4 and do some hyperparameter 
tuning to try to find the best performing model with all of these features***

![roc](/assets/log_reg_rocQ52.png)
{% include auc_52.html %}
{% include cumulative_perc_log_regQ52.html %}
{% include goal_rate_log_regQ52.html %}
![reliability](/assets/reliability_plotQ52.png)

To tune the hyperparameters (max depth, eta, and alpha), a bayesian search was performed. 
At the start of the simulation, we were using the accuracy as metric. However, since the data is known to 
be unbalanced and we found out early enough that the accuracy was not a good metric to use by itself in this situation. 
Therefore, we chose the f-score as metric. 
This choice will be detailed in section *Give it your best shot*.

We can see that the XGBoost model with all features has a slightly better AUC (.02 more). 
So we are slowly getting closer to a model that can correctly predict goals. 


The maximum probability (0.9) that this model outputs is higher than the baseline (0.7) and 
higher than any other model implemented so far. This can be seen on the calibration plots.
However, this model is not as well calibrated as the previous XGBoost.



Here is the code snippet for the tuning:

{% highlight ruby %}
config={
    "algorithm": "bayes",
    "spec": {
        "maxCombo": 0,
        "objective": "maximize",
        "metric": "f_score_val",
        "minSampleSize": 100,
        "retryLimit": 20,
        "retryAssignLimit": 0,
     },
     "parameters": {
            "max_depth": {"type": "integer", "min": 2, "max": 25},
            "eta": {"type": "float", "min": 0.1, "max": 1},
            "alpha": {"type": "float", "min": 0, "max": 1}
         },
    }
{% endhighlight %}


Here are the best hyper parameters resulting in a validation f-score of 0.195 and a validation accuracy of 0.889 
([link to experiment](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/b391850a36d846cda10125421f189e64?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)): 
- max_depth = 11,
- eta = 0.9101,
- alpha = 0.4451 . 


And here are the validation errors of the bayesian search: 
![bayesian_search_val_error](/assets/xgboost_val_error.png)
![bayesian_search_f_score](/assets/xgboost_f_score.png)

Link to the experiment with the logged model: [XGBoost tuned](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/7dbaa6b49896419fa2f2ffe91a35a044?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)



### Question 3
***Explore using some feature selection techniques to see if you can simplify your input features.***


To evaluate different features, we decided to use filter methods as wrapper methods would have been too computationally expensive. 
In that idea, we used chi-square, variance estimators (with threshold set at 0.1), anova test, and mutual information. 
We used cross validation though the process.

{% include feature_selection_table.html %}

Except for threshold variance, we used select k-best from sklearn with a parameter of 5 in order to see which 
features will be selected by the different algorithms.
While the results are similar, there is some disparity in the selected features indicating that we 
can not blindly select the features from these results.
*Note*: Due to the nature of these algorithms, we preferred to avoid encoding categorical features as either one-hot or ordinals as they would have had a mean and variance bias compared to the other features (shot_type and las_event_type).

To assess the performance of these subsets of features, we apply them to our previous (tuned) XGBoost model. 

Using SHAP we can see the impact of each feature on the prediction.



<u>Shap applied on XGBoost trained on features selected by variance threshold</u>
![shap](/assets/shap_plot_Variance Threshold.png)
<u>Shap applied on XGBoost trained on features selected by Chi-square</u>
![shap](/assets/shap_plot_Chi2.png)
<u>Shap applied on XGBoost trained on features selected by anova</u>
![shap](/assets/shap_plot_Anova.png)
<u>Shap applied on XGBoost trained on features selected by mutual information</u>
![shap](/assets/shap_plot_mutual_information.png)
<u>Shap applied on XGBoost trained with all features</u>
![shap](/assets/shap_plot_all.png)
<u>XGBoost features selection</u>
![shap](/assets/xgboost_feat_selection.png)


By analysing the table above and these SHAP, we were able to unselect some features and keep 
the following ones: 
- "game_seconds",
- "coord_y",
- "coord_x",
- "time_from_last_event",
- "shot_angle",
- "distance_from_last_event",
- "shot_distance",
- "rebound",
- "change_shot_angle",
- "goalie_rank",
- "shooter_rank".

Here are the plots of XGBoost with this subset:

![roc](/assets/log_reg_rocQ53.png)
{% include auc_53.html %}
{% include cumulative_perc_log_regQ53.html %}
{% include goal_rate_log_regQ53.html %}
![reliability](/assets/reliability_plotQ53.png)


This XGBoost model performs better than the previous ones. The calibration curve is very close to the perfectly calibrated benchmark. 
This time, the gain in AUC is much bigger (+0.044). The accuracy is 

From the goal rate plots, we can observe that the goal rate at the highest percentile is still 
bigger than the previous experiences (baseline: 22%, tuned XGBoost: 31%, XGBoost with subset: 38%). 

To this best subset, we also tried to add the following 3 categorical features (that we previously removed): 
- "team_info",
- "shot_type",
- "last_event_type",

These decisions were based on our domain knowledge and the previous milestone. 
It is known that different teams have different strategies and some teams are more aggressive which lead to more goals scored 
e.g. the Tampa bay Lightning team (185 more goals than the average on the training set).
We also observed in Milestone 1 that the shot type is strongly correlated to a shot being a goal or not. 

- *Link to comet experiment*: [XGBoost trained on best subset + 3 categorical features (added by domain knowledge)](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/de5a322e849c45d7b1144d326adb194e?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)


However, after experimenting with these additional subsets, we found out that the best AUC scores 
were obtained by not adding any of them (0.757 vs 0.769). So the most optimal set of features regarding the best AUC score (0.769) is 
the one described above. Its f-score is 0.119 on the validation set.


- *Link to comet experiment*: [XGBoost with best subset](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/8f18416d177141358e90e0f584979b6a?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)