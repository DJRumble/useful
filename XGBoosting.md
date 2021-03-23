# XGBoosting

XGBoost is an advanced gradient boosted tree algorithm. It is designed for parallel processing, regularization, early stopping which theoretically makes it a very fast, scalable and accurate algorithm. Upgrades over normal decisions trees are: 

Regularization: Standard GBT implementation has no regularization like XGBoost, therefore it also helps to reduce overfitting. In fact, XGBoost is also known as ‘regularized boosting‘ technique. 
Parallel Processing: XGBoost implements parallel processing and is therefore faster as compared to GBT.  High Flexibility XGBoost allow users to define custom optimization objectives and evaluation criteria. This adds a whole new dimension to the model and there is no limit to what we can do.  Handling Missing Values XGBoost has an in-built routine to handle missing values. The user is required to supply a different value than other observations and pass that as a parameter. XGBoost tries different things as it encounters a missing value on each node and learns which path to take for missing values in future. 
Tree Pruning: A GBT would stop splitting a node when it encounters a negative loss in the split. Thus it is more of a greedy algorithm. XGBoost on the other hand make splits up to the max_ depth specified and then start pruning the tree backwards and remove splits beyond which there is no positive gain. Another advantage is that sometimes a split of negative loss say -2 may be followed by a split of positive loss +10. GBT would stop as it encounters -2. But XGBoost will go deeper and it will see a combined effect of +8 of the split and keep both. 
 
Built-in Cross-Validation XGBoost allows user to run a cross-validation at each iteration of the boosting process and thus it is easy to get the exact optimum number of boosting iterations in a single run. This is unlike GBT where we have to run a grid-search and only a limited values can be tested. 

In a XGBoost we can tune: 
1. Booster Parameters Guide the individual booster (tree/regression) at each step 
2. Learning Task Parameters Guide the optimization performed

In addition to adjusting the maximum number of trees in XGBoost we can tune: 
1. Booster Parameters Guide the individual booster (tree/regression) at each step a) Controlling the model complexity (in suggested order of importance) Max number of trees, Max Depth, Min child weight, Gamma & L1, L2 regularization b) Robustness to noise Subsample, Colsample by tree 
2. Learning Task Parameters Guide the optimization performed Early stopping, Learning Rate, Missing Values

Booster Parameters 

A) Number of Trees - Increase values - Reduce bias but could increase variance (reduce underfitting) 
B) Early stopping - Use XGBoost built-in early stop mechanism so the exact number of trees will be optimized. Early stopping rounds (activated when Early Stopping is ticked) The optimizer stops if the loss never decreases for this consecutive number of iterations. 
C) Learning rate (eta) - Analogous to learning rate in GBT Decrease values Lower values are generally preferred as they make the model robust to the specific characteristics of tree and thus allowing it to generalize well. Lower values would require higher number of trees to model all the relations and will be computationally expensive.
D) L2 regularization term (lambda) on weights (analogous to Ridge regression)  - This used to handle the regularization part of XGBoost. Increase values Reduce overfitting
E) L1 regularization term (alpha) on weights (analogous to Lasso regression)  -Can be used in case of very high dimensionality so that the algorithm runs faster when implemented. Increase values Reduce overfitting
F) Gamma  -A node is split only when the resulting split gives a positive reduction in the loss function. Gamma specifies the minimum loss reduction required to make a split. Makes the algorithm more conservative. Increase values Reduce overfitting
G) Min Child Weight - Defines the minimum sum of weights of all observations required in a child. This is similar to Min Child Leaf size in GBT but not exactly. This refers to min “sum of weights” of observations while GBT has min “number of observations”. Used to control over-fitting.  Higher values prevent a model from learning relations which might be highly specific to the particular sample selected for a tree. Increase values Reduce overfitting
H) Subsample  - Same as the subsample of GBT. Denotes the fraction of observations to be randomly samples for each tree. Increase values Reduce underfitting
I) Colsample by tree Similar to max_features in GBT.  - Denotes the fraction of columns to be randomly samples for each tree. Increase values Reduce underfitting
J) Replace missing values  - You should not really be using this option as you should be controlling how to deal with missing values for each feature individually, not on a catch all scenario for every missing value across all features.

 

Typical Strategy 
1) Determine the optimum number of trees and learning rate using grid search 
2) Control Model complexity parameters in the following order: 
• Max depth 
• Min Child Weight 
• Gamma 
3) Add randomness to make training robust to noise: 
• Subsample 
• Colsample by tree 
3) Tune regularization parameters (lambda[L2], alpha [L1])

 