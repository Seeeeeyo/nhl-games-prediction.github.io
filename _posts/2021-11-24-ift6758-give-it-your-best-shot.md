---
layout: post
title: Give It Your Best Shot
---

## Give It Your Best Shot

### Question 1
***Now it's your turn to come up with the best model you can for predicting expected goals!***


To evaluate our models and experience in this section, we introduced a few more metrics to the AUC already discussed
in previous sections: 

- f-score 
- log-loss
- adjusted mutual information score

The f1 score uses 2 underlying metrics, precision and recall (true positive/number of goals).
It is the metric that we used all over the project to guide our bayesian searches and also to
compare every model produced. It is especially interesting since our 2 classes are very unevenly
represented (recall that there is ~10% of goals).  We quickly saw it's importance when we produced models
with 90% accuracy by predicting no goals at all.

The log-loss, both on the training and the test set was also used to prevent over/under-fitting.

The adjusted mutual information score will determine the correlation between our predictions 
and the true labels.

For this task, we tested the following models: 
- XGBoost,
- Neural network,
- Support Vectors Machines.

And the following techniques: 
- Oversampling,
- Undersampling,
- Scaling (min-max and standard scaler).

The first thing we tried in this section was scaling as we were going to use our most performing tool in this 
part in all the following experiences.  We used the min-max scaler.

We then needed to convert the categorical features ("shot type", the ranking of the goalers and shooters) 
into numerical values. We explored 3 methods to do so: 

- one-hot encoding
- ordinal encoding
- applying a metric to evaluate a score for a specific categorical feature

The ordinal encoding was introducing a bias in our model in the "shot type" feature as it would randomly 
select one of the shot type as the best and another as the worse.  While some linear model could make
more sense out of this, our models such as XGBoost, SVM and even our NN would have seen this gradient (from 
worse to best) as a feature by itself.
So in order to encode this feature properly, we went for the one-hot encoding.  
As for the ranking of the shooters and goalies, as explained in the feature engineering part of this blog,
we ranked them according to theirs goal/shots ratio and their save percentage respectively.
We also tried to add the team info in some way but the one-hot encoding for 32 teams (even 31 until the 2017
season) over-bloated our dataset. On top of adding 32 columns for the team tripling the size of our dataset 
since we found out that our best dataset had 15 features (including the encoding of shot type) it made little
sense to us to give so much weight to the team info.

In hindsight it would have been a good idea to proceed in the same way we did with the ranking of the players
to separate the 32 teams into groups with similar range of goals for and goals against.


Our data being so unevenly balanced between the goals and shots, the models tend to predict
only shots and no goals, so we thought that we could adjust our dataset to compensate this imbalance.
We therefore tried the over/under sampling methods.
The problem was that now that we trained our model with this new even dataset, our validation accuracy
crashed down since our model tried to predict way more goals than there actually was.  Interestingly,
even though the accuracy went down, our f1 score was improved from close to 0 in the first experiences 
to about 0.16 for all the models that we developed from that point on.
We can see the results of the sampling methods in the following figures.

Note that:
- *oversampling = False* means that we applied under sampling
- *oversampling = True* means that we applied over sampling
- *oversampling = None* means that we have not applied any resampling


![roc](/assets/log_reg_rocQ61.png)
{% include cumulative_perc_log_regQ61.html %}
{% include goal_rate_log_regQ61.html %}
{% include table_q6.html %}

As we can see, on top of testing the sampling techniques on XGBoost, we also applied them on a neural network and 
some tests were made on a SVM model but this last one's slow performances didn't allowed for a proper tuning of its
hyperparameters.

Our next experience was with SVM models.
We see a SVM produced without too much tuning gave interesting results especially with the AUC, we would 
have like to explore it furthermore with a complete bayesian search of hyperparameters but the nature of this model and 
its long training time didn't allowed us to push it to its limits. Especially, it would have been interesting to see the performance
of the svm model on the full dataset but the size of it didn't allowed our svm to train in a reasonable amount of time. This
is why we only see it trained on the undersample version of the training set, as this one is almost a tenth the size of the original
one.
We did however played a bit with this model, mostly with the different kernels available.  Scikit provides 3 main options 
regarding the kernel type:

- polynomial
- linear 
- RBF (radial basis function)

RBF caused a lot of crashing issues.  The linear one was explored a bit but as we thought, this classification problem
isn't linear by its nature and the polynomial stood out quite quickly, and indeed that's the kernel giving us the best result.


The next conducted set of experiences was with neural network models. 
Once again, a Bayesian search was performed to tune the hyperparameters.
The best neural network is the following: learning rate = 0.21944, 2 hidden layers, 
19 neurons on the 1st one and 18 on the second one, mini batch = 800. Although neural networks 
are known to be able to outperform other models, their complexity often requires lots of data that has been thoroughly 
cleaned. This might be a reason why our initial attempts have failed with a regular gradient descent and only the 
Adagrad optimizer gave us good results. It does a decent job but there is probably some room for fine tuning and improvements.

*Links to the experiments in comet:*
- [NN with oversampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/eecace91b768474ba4bb8562495115a1)
- [NN with downsampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/982dae82310141d798001dcadd57d63b)
- [NN without resampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/76e8c299594c4ca5a551a67dc8825937)
- [XGBoost with oversampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/e6ee7744286c45aea8515366237db8fc?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=step)
- [XGBoost with downsampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/22bdb664b9e2471ba46b9ee3eed95229)
- [XGBoost without resampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/c5647b4f533e4af99eb1dc5d89958b79)
- [SVM with undersampling](https://www.comet.ml/bariljeanfrancois/nhl-ds-milestone2/5423d15e091e494a95adcc389e5b70d5?experiment-tab=chart&showOutliers=true&smoothing=0&transformY=smoothing&xAxis=wall)