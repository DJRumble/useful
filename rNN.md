# An example of building a recuring Neural Network with Keras/Tensor flow

In this example we are going to a rNN to predict time series. 

### 1 - prepare data

Prepare a training and test set. Train on th ebulk of data and then test on the spefic time inteval (i.e. a  month). 
The Data should be a time series indexed sing column of volumes.

```
training = to_plot_no0[:'2019-10-01']['FOOTFALL']
actual = to_plot_no0['2019-10-01':'2019-11-01']['FOOTFALL']
```

![alt text](TS.png "Time Series Example")


### 2 - pre processing

https://scikit-learn.org/stable/modules/preprocessing.html

The sklearn.preprocessing package provides several common utility functions and transformer classes to change raw feature vectors into a representation that is more suitable for the downstream estimators.

Standardization of datasets is a common requirement for many machine learning estimators implemented in scikit-learn; they might behave badly if the individual features do not more or less look like standard normally distributed data: Gaussian with zero mean and unit variance.

An alternative standardization is scaling features to lie between a given minimum and maximum value, often between zero and one, or so that the maximum absolute value of each feature is scaled to unit size. This can be achieved using MinMaxScaler or MaxAbsScaler, respectively.

```
taget_colum=taget_colum.reshape(-1,1)
min_Max_scaler_target=preprocessing.MinMaxScaler(feature_range = (0,1))
training_taret = min_Max_scaler_target.fit_transform(taget_colum)
```

### 3 - 