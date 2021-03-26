# Introduction to neural networks and deep learning

### This wiki is based on the data camp online course [here](https://campus.datacamp.com/courses/deep-learning-in-python/basics-of-deep-learning-and-neural-networks?ex=3)

Linear models take a number features and use them to train a model *independantly of each other*. 
This is fine, but it makes the assumption that the features do *not* interact, which often is not the case. 
It is possible to calculate individual interactions between specific features within a linear model, to add depth. 
Alternatively you can use a  **neural network** to calculate these interactions automatically and enmase.
Networks are made of nurons, which are configured into layers of interactions, or **nodes**.

Within a neural network, these interactions are refered to as layers..
- Input layer - real data about tangible things in the real world
- hidden layer - interactions between input features that on their own are meaningless
- output layer - whatever the predictions are

Each interaction within the hidden layer is referred to as a **node** and are deffined by a number of **weighted features**. 

Training a deep learning network takes place iteratively, or in 'Epochs'. After initialise a base set of weights and biases, each Epoch contains the following process:
- Calculate the values of the output layer (the predicition) using forward propagation
- Calculate the error between the prediction values and the actual values
- Update the weights and biases using gradient descent and back propagation

Repeat this process for a given number of epochs or until our error is sufficently small.

### Forward Propagation

Forward propagation is the process through which data passes through layers of neurons in a neural network from the input layer all the way to the output layer, in short... 

1.  Start with the input layer as the input to the first hidden layer.
2.  Compute the weighted sum at the nodes of the current layer.
3.  Compute the output of the nodes of the current layer.
4.  Set the output of the current layer to be the input to the next layer.
5.  Move to the next layer in the network.
6.  Repeat steps 2 - 4 until we compute the output of the output layer.

Every node has connections to its surrounding nodes and these connections are regulated by a series of weights. Weightings are applied like a cross product. 
That is multiplication of the input data point times its node weigth, and then sumed across all input data points. These weights are key parameters in neural networks. 

```
#Layer one
z_11 = x_1 * w[0] + x_2 * w[1] + b[0]
z_12 = x_1 * w[2] + x_2 * w[3] + b[1]

#activation function  (sigmoid)
a_11 = 1.0 / (1.0 + np.exp(-z_11))
a_12 = 1.0 / (1.0 + np.exp(-z_12))

#Layer 2
z_2 = a_11 * w[4] + a_12 * w[5] + b[2]

#activation function  (sigmoid)
a_2 = 1.0 / (1.0 + np.exp(-z_2))

where:
x = input features
w = wieghts
b = additional factor or bias
z = sum of inputs
a = activation function(z)
```

In addition to weightings, we can also apply an **activation function** at each node to decide whether a nuron should be activated or not.
The reason to do this is better capature non-linear relationships in the data (without this this process is simply a linear regression).
After applying a cross product, we might apply a function to our output node value. 

- Sigmoid/tanh(x) - returns 1 at very large values and 0/-1 at very small values. However, suffers from the vanishing gradient problem where by at the extremes it will return very small changes in gradients, leading to much less efficent learning process.
- Relu - Returns a linear relationship at values greater than 0, but only returns the value 0 where it is less than 0. This both deals with the vanishing gradient problem, but also removes nudes with negative outputs from the learning process making it much more efficent. 

Traditionally the *tanh(x)* function was used but more recently industry has adopted the *ReLU* function which is a linear step function that looks like this. 


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

Building a forward propagation netowrk

```
n = 4 # number of inputs
num_hidden_layers = 3 # number of hidden layers
m = [2, 2,1] # number of nodes in each hidden layer
num_nodes_output = 1 # number of nodes in the output layer

def initialize_network(num_inputs, num_hidden_layers, num_nodes_hidden, num_nodes_output):
    
    num_nodes_previous = num_inputs # number of nodes in the previous layer

    network = {}
    
    # loop through each layer and randomly initialize the weights and biases associated with each layer
    for layer in range(num_hidden_layers + 1):
        
        if layer == num_hidden_layers:
            layer_name = 'output' # name last layer in the network output
            num_nodes = num_nodes_output
        else:
            layer_name = 'layer_{}'.format(layer + 1) # otherwise give the layer a number
            num_nodes = num_nodes_hidden[layer] 
        
        # initialize weights and bias for each node
        network[layer_name] = {}
        for node in range(num_nodes):
            node_name = 'node_{}'.format(node+1)
            network[layer_name][node_name] = {
                'weights': np.around(np.random.uniform(size=num_nodes_previous), decimals=2),
                'bias': np.around(np.random.uniform(size=1), decimals=2),
            }
    
        num_nodes_previous = num_nodes

    return network # return the network


def forward_propagate(network, inputs):
    
    layer_inputs = list(inputs) # start with the input layer as the input to the first hidden layer
    
    for layer in network:
        
        layer_data = network[layer]
        
        layer_outputs = [] 
        for layer_node in layer_data:
        
            node_data = layer_data[layer_node]
        
            # compute the weighted sum and the output of each node at the same time 
            node_output = node_activation(compute_weighted_sum(layer_inputs, node_data['weights'], node_data['bias']))
            layer_outputs.append(np.around(node_output[0], decimals=4))
            
        if layer != 'output':
            print('The outputs of the nodes in hidden layer number {} is {}'.format(layer.split('_')[1], layer_outputs))
    
        layer_inputs = layer_outputs # set the output of this layer to be the input to next layer

    network_predictions = layer_outputs
    return network_predictions

inputs = np.around(np.random.uniform(size=n), decimals=2)
my_network = initialize_network(n, num_hidden_layers, m, num_nodes_output)
predictions = forward_propagate(my_network, inputs)
print('The predicted values by the network for the given input are {}'.format(predictions))
```




### Back propagation & Gradient descent

**Backpropagation**  is a method used in artificial neural networks to calculate a gradient that is needed in the calculation of the weights to be used in the network. In effect it is the method by which we train the model. 
It has the following steps..

- Calculate the error – How far is your model output from the actual output.
- Minimum Error – Check whether the error is minimized or not.
- Update the parameters – If the error is huge then, update the parameters (weights and biases). After that again check the error. Repeat the process until the error becomes minimum.
- Model is ready to make a prediction – Once the error becomes minimum, you can feed some inputs to your model and it will produce the output.

When you are trying to train a neural network to predict accurately you will need to adjust the weightings to get as close as possible with your training data. 
The process of optimising these input parameters is refered to as  **gradient descent**. 
Gradient descent is a first-order iterative optimization algorithm for finding the minimum of a function. 
To find a local minimum of a function using gradient descent, one takes steps proportional to the negative of the gradient (or approximate gradient) of the function at the current point.

In effect you will use a metric (e.g. RMSE) to compare your prediction to the actual. 
Then move a direction in parameter space such that the error becomes smaller, untill it does not change any more (reaches a minimum). 
The magnitude of the move is often taken as the slope; however, this can be too large and therefore we apply a factor to reduce this in value. 
This factor is refered to as the  **learning rate** and is typically around 0.01.

A more sophisticated optimisation method 
is 'Adam'. Adam is more efficent as it does not need a predeffined learning rate.



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

## Deep learning

Deep networks take large numbers in input nodes and layers. 

### Convolution Neural Network

CNNs are comparable to those described above. 
However, they assume the inputs are image data, and incorporate a number of parameters specific to these use cases. The inputs are arrays on m*m*3 (a grid of pixels of three colours).
CNNs have many more specialised layers. 

- The **convolutional layer** applys a dot-product between a filter (kernel) and the original matrix to convolve the data down to a different scale. Convolution reduces the computational demands of dealing with 3 dimensions.
- The **pooling layer** reduces the spatial dimenstions of the data like the convolution. Rather than convolving the data, the pooling layer examines a subset of the matrix and keeps the maximum pixel value, reducing the scale again.
- The **fully connected layer** the reduced matrix is flattens the data into a vector and returns an output classification.

```

### Recurrent Neural Network

Traditional deep learning models assume input events are independent of each other. Recurrent NN over come this with loops that feedback from previous iterations. They are suitable for predicting sequences or time forecasting.
LSTMs are typical rNN method for forecasting future events. 

## Autoencoders

Autoencoders are unsupervised method for compressioning and decompressing data following patterns learned in an unsuperivsed manner. As a result, encoders are data specific. 
The autoencoder will use backpropagation to supply a target variable that is the same as the input. 
The encoder part functions in much the same way as a PCA, reducing the dimensionality of a set of input feature.

## 























