# K-means clustering

Nearest neighbours algorithms are “instance-based,” which means that save each training observation. They then make predictions for new observations by searching for the most similar training observations and pooling their values.  K Nearest Neighbour regression makes predictions for a sample by finding the k nearest samples and averaging their responses. This algorithm requires storing the entire training data into the model. Predictions may be slow.

Strengths: They are robust to noisy training data (especially if we use inverse square of weighted distance as the “distance”). They are effective if the training data is large but we have few features. 
Weaknesses: Harder to tune as we need to determine value of parameter K (number of nearest neighbours) and choose which type of distance based learning. Computation cost is quite high because we need to compute distance of each query instance to all training samples. Some indexing (e.g. K-D tree) may reduce this computational cost In practice, training regularized regression or tree ensembles are often better uses of your time.
 

A) K - The number of neighbours to examine for each sample. Increase values Reduce overfitting, Decrease values Reduce underfitting. Typical values: 2,4,8,16 
B) Distance weighting - If enabled, averaging across neighbours will be weighed by the inverse distance from the sample to the neighbour. Therefore closer neighbours will become more significant in predicting new values. I recommend enabling this. 
C) Neighbour finding algorithm - The method used to find the nearest neighbours to each point. It has no impact on predictive performance, but will have a high impact on training and prediction speed. 
• Automatic: a method will be selected empirically depending on the data. 
• KD & Ball Tree : stores the data points into a partitioned data structure for efficient lookup. 
• Brute force: will examine every training sample for every prediction. Usually inefficient. 
D) p  Which distance method to use. Options are:
• p = 2 gives the Euclidian distance 
• p = 1 gives the Manhattan distance. 
• p = 0 give the Hamming distance 
• p = large number gives the Chebyshev distance 

Euclidean is a good distance measure to use if the input variables are similar in type (e.g. all measured widths and heights). 
Manhattan distance is a good measure to use if the input variables are not similar in type (such as age, gender, height, etc.). If your data has high dimensionality it is suggested to use the Manhattan distance. However, in my experience it doesn’t make such a big difference. 

Typical Strategy Use grid search to find optimum K Value. Turn on Distance weighting. Then tune Distance method (p) if you have time.

