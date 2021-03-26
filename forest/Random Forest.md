# Random forests

Regression trees (a.k.a. decision trees) learn in a hierarchical fashion by repeatedly splitting your dataset into separate branches that maximize the information gain of each split. This branching structure allows regression trees to naturally learn non-linear relationships.  Ensemble methods, such as Random Forests (RF), XGBoost (XG), Gradient Boosted Trees (GBT), combine predictions from many individual trees. In practice, RF’s often perform very well out-of-the-box while XG’s & GBT’s are harder to tune but tend to have higher performance ceilings. 

Strengths: Decision trees can learn non-linear relationships, and are fairly robust to outliers. Ensembles perform very well in practice, winning many classical (i.e. non-deep-learning) machine learning competitions. Capable of fitting a large number of functional forms. No assumptions (or weak assumptions) about the underlying function. 
Weaknesses: Unconstrained, individual trees are prone to overfitting because they can keep branching until they memorize the training data (however, this can be alleviated by using ensembles). Are unable to predict a target beyond the current scope of the data. They sometimes can be lot slower to train as they often have far more parameters to train but again this can be mitigated by using different algorithms (e.g.. XGBoost).

They share parameters with single decision trees. But in both algorithms we can also control: 

A) Number of Trees 
Increase values - Reduce bias but could increase variance (reduce underfitting) 
Decrease values - Increase bias but could decrease variance (reduce overfitting) 
Recommended values (if not default) - A typical range is between 100-1000. Usually changing this value will have the greatest effect on under/ overfitting. 

B) Feature Sampling 
Strategy - Adjusts the number of features to sample at each split 
Automatic - Will select 30% of the features Square root Select the square root of the number of features 
Logarithm - Select the base 2 logarithm of the number of features Fixed number Select the given number of features Fixed proportion - Select the given proportion of feature 
Recommended values (if not default) - This depends on the size of your dataset, the number of features and how many trees you select to have. If your model is underfitting you should increase the number of features that are being sampled. If your model is overfitting you should reduce the number of features that are being sampled included. If your model is taking a long time to process you should reduce the number of features that are being included. 
 
Typical Strategy - The number of trees, followed by depth of the trees and the minimum samples per spilt usually have the biggest potential corrective effect. Incremental gains could be gained by changing the feature sampling strategy, cost function or spilt strategy (if you have the time).