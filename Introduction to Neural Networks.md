# Introduction to neural networks

### This wiki is based on the data camp online course [here](https://campus.datacamp.com/courses/deep-learning-in-python/basics-of-deep-learning-and-neural-networks?ex=3)

Linear models take a number features and use them to train a model *independantly of each other*. This is fine, but it makes the assumption that the features do *not* interact, which often is not the case. It is possible to calculate individual interactions between specific features within a linear model, to add depth. Alternatively you can use a  **neural network** to calculate these interactions automatically and enmase.
 

Within a neural network, these interactions are refered to as layers..
- Input layer - real data about tangible things in the real world
- hidden layer - interactions between input features that on their own are meaningless
- output layer - whatever the predictions are

### Forward Propagation
Each interaction within the hidden layer is referred to as a **node**. They are deffined by a number of **weighted features**. These weights are key parameters in neural networks. Weightings are applied like a cross product. That is multiplication of the input data point times its node weigth, and then sumed across all input data points.
In addition to weightings, we can also apply an **activation function** at each node. The reason to do this is better capature non-linear relationships in the data. After applying a cross product, we might apply a function to our node value. Traditionally the *tanh(x)* function was used but more recently industry has adopted the *ReLU* function which is a linear step function that looks like this. The Activation function is applied to each node and also to the output. We don't have to stop at one layer. More powerful, or dee


```
input_data = array([3, 5])
weights = {'node_0': array([2, 4]), 'node_1': array([ 4, -5]), 'output': array([2, 7])}

def relu(input):
    '''Define your relu activation function here'''
    # Calculate the value for the output of the relu function: output
    output = max(0, input)
    # Return the value just calculated
    return(output)

# Calculate node_0_input
node_0_input = (input_data * weights['node_0']).sum()

# Apply activation function
node_0_output = relu(node_0_input)

# Calculate node_1_input
node_1_input = (input_data*weights['node_1']).sum()

# Apply activation function
node_1_output = relu(node_1_input)

# Put node values into array: hidden_layer_outputs
hidden_layer_outputs = np.array([node_0_output, node_1_output])

# Calculate output: output
input_to_final_layer = (hidden_layer_outputs*weights['output']).sum()

# Apply activation function
model_output = relu(input_to_final_layer)
```

### Back propagation & Gradient descent

**Backpropagation**  is a method used in artificial neural networks to calculate a gradient that is needed in the calculation of the weights to be used in the network. In effect it is the method by which we train the model. It has the following steps..

- Calculate the error – How far is your model output from the actual output.
- Minimum Error – Check whether the error is minimized or not.
- Update the parameters – If the error is huge then, update the parameters (weights and biases). After that again check the error. Repeat the process until the error becomes minimum.
- Model is ready to make a prediction – Once the error becomes minimum, you can feed some inputs to your model and it will produce the output.

When you are trying to train a neural network to predict accurately you will need to adjust the weightings to get as close as possible with your training data. The process of optimising these input parameters is refered to as  **gradient descent** . 
> Gradient descent is a first-order iterative optimization algorithm for finding the minimum of a function. To find a local minimum of a function using gradient descent, one takes steps proportional to the negative of the gradient (or approximate gradient) of the function at the current point.

In effect you will use a metric (e.g. RMSE) to compare your prediction to the actual. Then move a direction in parameter space such that the error becomes smaller, untill it does not change any more (reaches a minimum). The magnitude of the move is often taken as the slope; however, this can be too large and therefore we apply a factor to reduce this in value. This factor is refered to as the  **learning rate** and is typically around 0.01.

## Calculating an error

```error = preds - target```

The slope is a first order derivative, examining where the rate of change of the error, wrt weight, tends to zero. Simply this can be stated as...

## Deffinition of slope

```slope = 2 * input_data * error```

In more depth this is..

```d(e)/dW = d(e)/dPred* d(Pred)/dIn * d(In)/dW```

where..

```d(e)/dPred ```= rate of change of the error, wrt the prediction. Essentially this is ```preds - target```
```d(Pred)/dIn``` = rate of change of the prediction, wrt the input. Essentially this is ```preds (1-preds)``` [??]
```d(In)/dW``` = rate of change of the input, wrt to the weight. Essentially this is the cross products of the inputs and the weights.

Updating weights from a slope

```weights_updated = weights - (slope*learning_rate)```

The Backpropagation algorithm looks for the minimum value of the error function in weight-parameter space using gradient descent. This process is repeated until then slope = 0. In this way we back propagate from the predicition to gain an improved result.