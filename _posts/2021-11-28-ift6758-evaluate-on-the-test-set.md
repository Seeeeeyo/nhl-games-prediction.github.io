---
layout: post
title: Evaluate on test set 
---

## Evaluate on test set 
***Evaluate these models on the holdout test set you set aside in part 1.***
### Question 1
***Test your 5 models on the untouched 2019/20 regular season dataset. Do your models perform as well on the test set as you did on your validation set when building 
your models? Do any models perform better or worse than you expected?***


Here are the graphs of the models with the specified test data:
![roc](/assets/log_reg_rocQ71.png)
{% include goal_rate_log_regQ71.html %}
{% include cumulative_perc_log_regQ71.html %}
![rel](/assets/reliability_plotQ71.png)

 {% include table_71.html %}

We can see that our 2 preferred models keep on performing well on their respective best scores. As we discussed
before, the logistic regression models are still predicting only shots to yield about 90% accuracy but f-scores of 0.
There are a few factors to consider when analyzing these results. the XGBoost is still performing pretty well 
in the reported metrics. As there was no game of the 2019 season in the training set, 
we are generating average ranks for all new rookie players. Since we saw previously that the 2 ranking
features that we created were important, it is to be expected that we would lose a bit of precision with all the
events concerning new shooters and new goalies. We also know that the 2019-2020 season was shorter than usual due 
to Covid-19, so we don't have the same representativity of a whole season than what would have
had for a full normal season.
As reported in the table above, the best accuracy is 0.91 and the best AUC is 0.757. Both are from the neural network.
This model outputs more or less the same metrics as on the validation set. 
The best f-score (0.268) was obtained with svm. This was already the case on the validation set.  


### Question 2
***Test your 5 models on the untouched 2019/20 playoff games. Discuss your results and observations on this test set. 
Are there any differences to the regular season test set or do you get similar ‘generalization’ performance?***

 This test set is very different from the training set for 2 main reasons:
- the post-season games have a few different rules as games will go on overtime until a team scores a goal.
- the playoffs of the 2019-2020 were played during in August (!), after a 5 months break (!!)

So just between those two points, it is to be expected that our models might not perform as well as they would 
with a test set of a regular season.

Here are the graphs of the models with the specified test data:
![roc](/assets/log_reg_rocQ72.png)
{% include goal_rate_log_regQ72.html %}
{% include cumulative_perc_log_regQ72.html %}
![rel](/assets/reliability_plotQ72.png)

 {% include table_72.html %}
 
 
 The results are almost exactly the same as with the regular season (question above):
 - The best models are still NN for AUC and accuracy, and svm for f-score.
 - Logistic regression models keep predicting only shots.
 - XGBoost is still very good in all metrics.
 
 It is quite surprising to observe these since there are already many differences between the 2 test sets and the training set.
 Furthermore, considering the multiple particularities of 2019-2020 playoffs games, 
 it differs even more from the original training set. 
 However, this means that the models have a good generalization.

