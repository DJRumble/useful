# Long-Short-Term-Memory

LSTM is a form of recurrent neural network that is useful for modelling sequential data. 
A bidirectional rNN can make use of past, present and future information, and provide the ability to understand the wider context of a string of actions or events (i.e. thier interdependency).

As with all Neural Networks, there is an **input layer**. In this case the number of nodes in the input layer is equal to the maximum length of your training sequence.

```input_word = Input(shape=(max_len,))```

Typically at this point ypou want to include an **embedding layer**, which embeds input dimensions equal to the number of unique events in your sequence into dimensions equal to your sequnece lenght (or input layer).

```model = Embedding(input_dim=num_words, output_dim=50, input_length=max_len)(input_word)```

You may want to apply a **droput layer**. dropout refers to ignoring units (i.e. neurons) during the training phase of certain set of neurons which is chosen at random. By “ignoring”, I mean these units are not considered during a particular forward or backward pass. 
Including a dropout layer helps to reduce overfitting as a fully connected layer occupies most of the parameters, and hence, neurons develop co-dependency amongst each other during training which curbs the individual power of each neuron leading to over-fitting of training data.
- Dropout forces a neural network to learn more robust features that are useful in conjunction with many different random subsets of the other neurons.
- Dropout roughly doubles the number of iterations required to converge. However, training time for each epoch is less.

SpatialDroppout goes a step further and drops the 1D feature map across the channels(??)

```model = SpatialDropout1D(0.1)(model)```

A special **LSTM layer** needs to be included. This can be phrased in a Bidirectional form that will all the training to account for the full rang of the sequnece as opposed to just the most recent entry.
LSTM hyper params include:
- units - larger = longer to train and more accurate]
- recurrent drop out - see above
- For full list go to this link - https://keras.io/api/layers/recurrent_layers/lstm/

```model = Bidirectional(LSTM(units=100, return_sequences=True, recurrent_dropout=0.1))(model)```

In addition, you may wish to invoke a **TimeDistributed layer** which will apply a **Dense layer** at each time point in the sequence, not just the most recent event.

```out = TimeDistributed(Dense(num_tags, activation="softmax"))(model)
```

You will need the **output dense layer** which will in this case will have dimensions/nodes equal to the number of categories we are trying to predict.

```model = Model(input_word, out)```

Finally compile and optimise with **model.compile**

```
model.compile(optimizer="adam",
              loss="sparse_categorical_crossentropy",
              metrics=["accuracy"])
```
