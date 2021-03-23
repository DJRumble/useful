# Keras/tensor flow

Google have done a very useful walk through behind setup up a neural network using TensorFlow, complete with an archive of code. Check out these links.

- https://github.com/martin-gorner/tensorflow-mnist-tutorial
- https://codelabs.developers.google.com/codelabs/cloud-tensorflow-mnist/#0

A Keras/Tensor flow model has the four following compenents to consider..

- Specifiying a model (layers, nodes, Activation functions)
- Compile the model (deffine the loss function, learning rate, optimizer)
- Fit the model (backpropagation and training
- Scoring

### Specifiying a model

```
# Import necessary modules
import keras
from keras.layers import Dense
from keras.models import Sequential

# Save the number of columns in predictors: n_cols
n_cols = predictors.shape[1]

# Set up the model: model
model = Sequential()

# Add the first layer
model.add(Dense(50, activation='relu', input_shape=(n_cols,)))

# Add the second layer
model.add(Dense(32, activation='relu'))

# Add the output layer
model.add(Dense(1))

```
### Compiling and fitting

```

# Compile the model - REGRESSION
model.compile(optimizer='adam', loss='mean_squared_error')

# Compile the model - CLASSIFCATION
model.compile(optimizer='sgd',loss='categorical_crossentropy',metrics=['accuracy'])

# Fit the model
model.fit(predictors,target)

```

Note that the loss function varies depending on whether the problem is classification or regression..

- Regression | mean_quared_error
- Classifcation | Log-Loss

Models can be saved and recalled for later on.

### Model optimization

Conducting grid search over a range of features.

- Dying nuron - when outcomes are consistently on the negative side of the ReLU function and therefore are propagated as '0'
- Vanishing gradient - when using a tanh fucntion, over many layers outcomes tend to zero at a great computational cost.

Features to grid search -
- Learning rate ```optimizer=SGD(lr=lr)```

### Model Validation and capacity

Various validation methods can be applied to the ```model.fit()``` process through a number of arguements.
- ```validation_split=0.3``` - will split out some of the data for validation
- ```EarlyStopping(patience=2)``` - will terminate the number model run when it is no longer improving (patience is the number of iterations to try before stopping early)
- ```epochs=30``` - the number of iterations to try with the model before automatically stopping.

Full example...
```
# The input shape to use in the first hidden layer
input_shape = (n_cols,)
# Create the new model: model_2
model_2 = Sequential()

# Add the first, second, and third hidden layers
model_2.add(Dense(50, activation='relu',input_shape=input_shape))
model_2.add(Dense(50, activation='relu'))
model_2.add(Dense(50, activation='relu'))

# Add the output layer
model_2.add(Dense(2,activation='softmax'))

# Compile model_2
model_2.compile(optimizer='adam',loss='categorical_crossentropy',meterics=['accuraccy'])

# Fit model 1
model_1_training = model_1.fit(predictors, target, epochs=20, validation_split=0.4, callbacks=[early_stopping_monitor], verbose=False)

# Fit model 2
model_2_training = model_2.fit(predictors, target, epochs=20, validation_split=0.4, callbacks=[early_stopping_monitor], verbose=False)

# Create the plot
plt.plot(model_1_training.history['val_loss'], 'r', model_2_training.history['val_loss'], 'b')
plt.xlabel('Epochs')
plt.ylabel('Validation score')
plt.show()
```
No need to use K-fold validation.

#### Capacity

Large capacity = more nodes/layers

Increasing capacity can improve the model but after a while you may risk over fitting, at which point performance will drop off. Conducting sequentail experiments will allow you to optimise the most effective use of resources. 

### Image predictions
The MNIST data set is a popular image dataset for learning about image recognication. They are pictures of numbers on a 28x28 pixel grid. Here is a code that can make predictions on images at 98% accuracy!

```

# Create the model: model
model = Sequential()

# Add the first hidden layer
model.add(Dense(50,activation='relu',input_shape=(784,)))

# Add the second hidden layer
model.add(Dense(50,activation='relu'))

# Add the output layer
model.add(Dense(10,activation='softmax'))

# Compile the model
model.compile(optimizer='adam',loss='categorical_crossentropy',metrics=['accuracy'])

# Fit the model
model.fit(X,y,validation_split=0.3)

```