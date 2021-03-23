# Validation metrics

## Classification metrics

[Recall ](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)- The recall is intuitively the ability of the classifier to find all the positive samples - i.e. the number of positives that are correctly predicted. [tp / (tp + fn) where tp is the number of true positives and fn the number of false negatives.]
[Precision](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html) - The precision is intuitively the ability of the classifier not to label as positive a sample that is negative - i.e. the number of positives that are correct. [The precision is the ratio tp / (tp + fp) where tp is the number of true positives and fp the number of false positives.]
[F1 score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html) - The F1 score can be interpreted as a weighted average of the precision and recall. [F1 = 2 * (precision * recall) / (precision + recall)]
[AUC score](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.auc.html#sklearn.metrics.auc)/[ROC AUC score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html#sklearn.metrics.roc_auc_score) - Compute Area Under the Curve (AUC) from prediction scores. Requires a complete ROC Curve to work properly.

Gini coefficient - (2*AUC)-1

## Regression metrics

[R2](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.r2_score.html) - coefficient of determination
[MSE ](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)- Mean Squared Error 
[Chi2](http://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.chi2.html) - Chi Squared statistics

## Concept of Gains/ROC Curves

**X-axis:**  the cumulative proportion of the universe included in the sample (exposure)/ the FPR for cumulative bins.
**Y-axis: ** The cumulative proportion of sales/responses attributed to the selected population (response)/the TPR for cumulative bins.

In both cases, a model returns a prediction and a propensity score. The 'universe' can then be sorted by propensity score and sampled in bins (often deciles)


Gains curves are used to validate models. These partition the model results into deciles and the cumulative bins are plotted for a range of scenarios:
 -  **Ideal (green): ** in an ideal scenario, all the positive results are sorted into the top bins. Therefore the percentage response rate quickly reaches 100%. Complete segregation.
 -  **Random (red):**  results are sorted randomly and therefore percentage response has a 1:1 relationship with population size. No discrimination.
 -  **Model (blue):**  our model will fall in between with imperfect sorting of results into the upper bins. 

Receiver Operating Characteristic (ROC) curves are a visual aid for measuring the predictive performance of a binary classification model. The curve is created by plotting the true positive rate (TPR, sensitivity) against the false positive rate (FPR) at various threshold settings.
Each population bin will have a number of correct and incorrect predictions. When added cumulatively, these make up the TPR and FPR. Plotting each discrete bin as a point will trace out a function, to which a continuous curve can be fitted. 

```http://gim.unmc.edu/dxtests/roc2.htm ```
```http://www.anaesthetist.com/mnm/stats/roc/Findex.htm ```

**Model comparisons** 

```http://www.business-insight.com/blog/2016/07/lift-roc-auc-and-gini/ ```

Curves can also be used to assess the productiveness of different models. By again ordering the observations by fitted value (highest first) we can plot cumulative actual number of claims against cumulative exposure (see Chart above/below). Models can be assessed (against the mean model) by the speed at which the actual number of responses accumulates. In a predictive model, those policies with the largest observed frequencies should contribute to the left of the graph, as the observations have been ordered by fitted value. In other words, if the model is predictive, high fitted values should correspond to high observed values. 

In Chart A above two models are compared – here Model A appears to be more predictive (ignoring issues such as over-parameterisation etc.) than Model B as the curve for the former is always above that for the latter. 


## Gini coefficient

A Gini coefficient is a measure of the area under the curve (AUC) for a model — in this case the Gini coefficient is defined as Gini=(2*AUC)-1. The straight line on the graphs above corresponds to the random (mean) model and has a Gini coefficient of 0. Under the definition given above, the Gini coefficient can take values in the range {-1, 1} — a larger positive Gini coefficient means that the model is more predictive.
The weighted Gini coefficient for each fitted model results plotted is displayed in the legend of the graph. This value is a measure of the difference between the fitted model and the mean model.


**Validation criterion **

Is a model effective?  -- Gini coef. > 0.8
Is a model reliable (i.e. not over fitting)?  --Ginitest/Ginitrain > 0.95


Calculation (numerical integration)

The Gini, under definition 2, Gini = (2 * AUC) - 1 is calculated in Emblem by numerical integration. This is calculated as:
