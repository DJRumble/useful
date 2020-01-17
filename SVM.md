# Support vector machines (SVM) 

Support vector machines (SVM) use a mechanism called kernels, which essentially calculate distance between two observations. The SVM algorithm then finds a decision boundary that maximizes the distance between the closest members of separate classes.  For example, an SVM with a linear kernel is similar to logistic regression. Therefore, in practice, the benefit of SVM’s typically comes from using non-linear kernels to model non-linear decision boundaries. 


Strengths: SVM’s can model non-linear decision boundaries, and there are many kernels to choose from. They are also fairly robust against overfitting, especially in high-dimensional space. 
Weaknesses: However, SVM’s are memory intensive, trickier to tune due to the importance of picking the right kernel, and don’t scale well to larger datasets. Currently to date, random forests and other CART ensembles are usually preferred over SVM’s because of this. 

A) Kernel type -Unless you have a prior understanding of how your data is structured theoretically you should select all options and let Grid Search find the best fitting Kernal. However, the default RBF kernal is often the best option for non-linear models.
B) C (default 1)  - A tuning parameter is introduced called simply C that defines the magnitude of the wiggle allowed across all dimensions. The C parameters defines the amount of violation of the margin allowed. Increase values Reduce underfitting. Start at 1 (default) and depending on test/ hold-out results in re. under/ overfitting adjust the direction of tuned value accordingly
C) Gamma (Kernel coefficient for RBF, polynomial and sigmoid kernels only). If gamma is zero/ default, then 1/n_features will be used instead. Increase values Reduce underfitting. I would leave as zero and use the default 1/n_features value. If you want to increase or decrease use the same range of values as B) C.
D) Tolerance Tolerance for stopping criterion. Specifies the maximum gradient of the quadratic function used to compute support vectors. Training is terminated when the gradient of the optimized function is less than or equal to the tolerance value. 
E) Maximum number of iterations (stopping criteria before convergence of model) 

 If your model keeps on running without stopping it is probably a sign that you don’t have a high performing model. To stop your model running forever best to put a max value here. Recommended values -1 = unlimited is the default which I wouldn’t initially change. A large value like 1000 could be inputted as a max value but in Dataiku I would just abort the algorithm if it was still running after a long period of time.