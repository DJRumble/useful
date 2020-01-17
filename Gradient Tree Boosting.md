# Gradient Boosting

 Ensemble meta-algorithm (applied to a set of machine learning algorithms) used to convert/boost many weak learners (decision trees) into a strong learner. 

Weak learner - slightly better than random
Strong learner - a tuned learner that is arbitrarily good

Boosting is the result of:

Iteratively learning a set of weak models on subsets of data
Weighing each weak prediction according to each weak learners performance
Combine the weighted predictions to a single weighted prediction that is much better than the individual.
Greater than the sum of its parts
 

A) Number of Boosting Stages (Trees/ n_estimators) 

Increase values - Reduce bias but could increase variance (reduce underfitting). Will increase processing time 
Decrease values - Increase bias but could decrease variance (reduce overfitting) 
Recommended values (if not default) - A typical range is between 100-1000. Start with 100 and depending on the levels of bias and variance adjust accordingly. Usually changing this will have the greatest effect on under/ over-fitting. 

B) Learning rate 

Decrease values - Lower values are generally preferred as they make the model robust to the specific characteristics of tree and thus allowing it to generalize well. Lower values would require higher number of trees to model all the relations and will be computationally expensive. 
Increase values - Will reduce computation time but should only apply if you have a large amount of data. 
Recommended values (if not default) - A typical range is between 0.01-0.1. One formula you can apply is: [2-10] / Number of Boosting Stages (Trees) 

C) Loss (where the Target is categorical) 

Try Exponential (AdaBoost method) - This usually performs better but will take longer to process than below. 
Try Deviance (logistic regression method) - Is less computationally intensive (faster) but not robust to outliers as it uses the squared error. 
Recommended values (if not default) - If you have time, select both and let the in-built grid search function to find the best performing option but in the vast majority of instances it will not matter. 

Typical Strategy - As with other ensemble decision tree based algorithms the number of boosting stages (trees) followed by depth of the trees usually have the biggest potential corrective effect. Potentially the learning rate might need to be reduced to correct underfitting but usually the default rate should be ok, otherwise use the suggested formula. Incremental gains could be gained by changing cost function if you have the time.

 