# Deep learning in Keras

- Tensor Flow (most popular)
- Keras (Most accessible, built on Tensor Flow)
- PyTorch (up and coming)

A classical deep NN is built in Keras using the `sequential' package.

## Regressions model
```
import keras
from keras.models import Sequential
from keras.layers import Dense

model = Sequential()

n_cols = data.shape[1]

#hidden layer 1
model.add(Dense(5,activation='relu', input_shape=(n_cols,)))
#hidden layer 2
model.add(Dense(5,activation='relu'))
#output layer
model.add(Dense(1))

#deffine backpropagation methods
model.compile(optimizer='adam', loss='mean_squared_error')

#train and predict on test
model.fit(predictors,target)

predictions = model.predict(test_data)
```

## Multi-classification model

The main difference here is that the output layer has nodes equal to the number of categories, as opposed to just one float value

```
import keras
from keras.models import Sequential
from keras.layers import Dense
from keras.utils import to_categorical

model = Sequential()

n_cols = data.shape[1]

target= to_categorical(target)

#hidden layer 1
model.add(Dense(5,activation='relu', input_shape=(n_cols,)))
#hidden layer 2
model.add(Dense(5,activation='relu'))
#output layer
model.add(Dense(4, activation='softmax'))

#deffine backpropagation methods
model.compile(optimizer='adam', 
	      loss='categorical_crossentropy',
	      metrics=['accuracy'])

#train and predict on test
model.fit(predictors,target,epochs=10)

predictions = model.predict(test_data)


```

## Convolutional Network

```
model = Sequential()

input_shape = (128,128,3)

num_classes = 10

    # create model
    model = Sequential()
    model.add(Conv2D(16, (5, 5), activation='relu', input_shape=input_shape))
    model.add(MaxPooling2D(pool_size=(2, 2), strides=(2, 2)))
    
    model.add(Conv2D(8, (2, 2), activation='relu'))
    model.add(MaxPooling2D(pool_size=(2, 2), strides=(2, 2)))
    
    model.add(Flatten())
    model.add(Dense(100, activation='relu'))
    model.add(Dense(num_classes, activation='softmax'))
    
    # Compile model
    model.compile(optimizer='adam', loss='categorical_crossentropy',  metrics=['accuracy'])```









 