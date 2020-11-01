---
layout: content
title: "Matrix Operations and Neural Networks"
date: 2020-11-01 00:00:00
description: Representation of neurons and connections using linear algebra
---

A [video](https://www.youtube.com/watch?v=UNmqTiOnRfg) by Luis Serrano provides an introduction to recurrent neural networks, including the mathematical representations of neural networks using linear algebra. [This article](https://towardsdatascience.com/linear-algebra-explained-in-the-context-of-deep-learning-8fcb8fca1494) also provides some example of using matrices as a model for neural networks in deep learning.

Suppose we have the following neural network.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,4.25) node {Input Layer};
    \draw (5.5,4.25) node {Hidden Layer};
    \draw (9.5,4.25) node {Output Layer};

    \draw (0,0) rectangle (3,4);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,4);
    \draw (5.5,3.25) circle (0.2in);
    \draw (5.5,2) circle (0.2in);
    \draw (5.5,0.75) circle (0.2in);

    \draw (8,0) rectangle (11,4);
    \draw (9.5,2.5) circle (0.2in);
    \draw (9.5,1.25) circle (0.2in);

    \draw [->] (2,3.25) -- (5,3.25);
    \draw [->] (2,3.25) -- (5,2);
    \draw [->] (2,3.25) -- (5,0.75);

    \draw [->] (2,2) -- (5,3.25);
    \draw [->] (2,2) -- (5,2);
    \draw [->] (2,2) -- (5,0.75);

    \draw [->] (2,0.75) -- (5,3.25);
    \draw [->] (2,0.75) -- (5,2);
    \draw [->] (2,0.75) -- (5,0.75);

    \draw [->] (6,3.25) -- (9,2.5);
    \draw [->] (6,3.25) -- (9,1.25);

    \draw [->] (6,2) -- (9,2.5);
    \draw [->] (6,2) -- (9,1.25);

    \draw [->] (6,0.75) -- (9,2.5);
    \draw [->] (6,0.75) -- (9,1.25);
  \end{tikzpicture}
</script>

Suppose we're expecting input vector $$x$$ and output vector $$y$$, where $$x$$ and $$y$$ defined as follows.

$$
x = \left[
  \begin{matrix}
  x_{1} \\
  x_{2} \\
  x_{3}
  \end{matrix}
\right] \\
y = \left[
  \begin{matrix}
  y_{1} \\
  y_{2}
  \end{matrix}
\right]
$$

We can see that the input nodes are connected to the hidden nodes. We have 3 input nodes and 3 hidden nodes. The connection weights between the input and hidden nodes can be represented as an $$m \times n$$ matrix where $$m$$ is the number of the hidden nodes and $$n$$ is the number of the input nodes.

$$
W_{i \to h} = \left[
  \begin{matrix}
  w_{i \to h 1,1} & w_{i \to h 1,2} & w_{i \to h 1,3} \\
  w_{i \to h 2,1} & w_{i \to h 2,2} & w_{i \to h 2,3} \\
  w_{i \to h 3,1} & w_{i \to h 3,2} & w_{i \to h 3,3}
  \end{matrix}
\right]
$$

The connection weights between the hidden nodes and the output nodes can be represented as follows.

$$
W_{h \to o} = \left[
  \begin{matrix}
  w_{h \to o 1,1} & w_{h \to o 1,2} & w_{h \to o 1,3} \\
  w_{h \to o 2,1} & w_{h \to o 2,2} & w_{h \to o 2,3}
  \end{matrix}
\right]
$$

Therefore, the process of the neural network can be represented by the following equation.

$$
y = W_{h \to o} W_{i \to h} x
$$

When training the network, we're looking for a set of weight matrices that can give us the most fitting output vector $$y$$ given the input vector $$x$$ from our training data.

# Activation Function and Bias

As described in the previous section, we can model the neural network drawn in the beginning of this article as the following matrix operation.

$$
y = W_{h \to o} W_{i \to h} x
$$

Where the values of the hidden nodes can be calculated as follows.

$$
H = W_{i \to h} x
$$

With the operations above, each time the values are fed from one layer to the next layer we only need to calculate the product of the weight matrix for the interlayer connection and the vector we're passing to the next layer to get the resulting matrix. But in a lot of cases, we might want to preprocess the result vector to ensure the values in the vector falls into a certain range according to our needs for the next step of the computation process. Here, we can use an [activation function](https://en.wikipedia.org/wiki/Activation_function) on the resulting vector to ensure the values contained in the vector falls into a certain range that's suitable for our use case.

Suppose $$f_{i \to h}$$ is the activation function of the hidden layer $$H$$, the matrix operation for the input vector $$x$$ on the hidden layer will be as follows.

$$
H = f_{i \to h}(W_{i \to h} x)
$$

Another method we can use to modify the resulting vector to make it fit better for further computation is by using [bias](https://missinglink.ai/guides/neural-network-concepts/neural-network-bias-bias-neuron-overfitting-underfitting/). Using bias, we can perform addition (or subtraction, for negative bias value) on the resulting vector values for the layer in order to make adjustments to the vector values.

Suppose $$B_{i \to h}$$ is the vector of bias values for the hidden layer $$H$$, the matrix operation for the input vector $$x$$ on the hidden layer will be as follows.

$$
H = f_{i \to h}(W_{i \to h} x + B_{i \to h})
$$

# Backpropagation and Gradient Descent

[Backpropagation](http://neuralnetworksanddeeplearning.com/chap2.html) is one of the most important algorithm in optimizing a neural network's parameters during the training process in order to reduce the model's error by finding the configuration of parameters resulting with the minimum error value. Backpropagation is meant to help with [gradient descent](https://towardsdatascience.com/understanding-the-mathematics-behind-gradient-descent-dde5dc9be06e) in the context of a neural network.

[This video](https://www.youtube.com/watch?v=Ilg3gGewQ5U) gives an explanation on backpropagation with clear visuals. The backpropagation algorithm basically reverse the process of the matrix operations from the input layer to output layer in order to find the weight and bias adjustments the neural network needs to make in order to have the output vector fits the actual data label during the training process. [This article](https://brilliant.org/wiki/backpropagation/) also gives a good explanation of backpropagation along with some relevant formulas to be used in the computation process.

During the training process, the backpropagation algorithm is used to calculate the difference between the actual output and the expected output on the interlayer matrix operations of the neural network and update the weights accordingly so that the matrix operations in the neural network can give us output vectors with smallest possible error if compared to the expected output according to the designated error function.

# Extra Notes

[This resource](https://www.whitman.edu/Documents/Academics/Mathematics/2016/Schafer.pdf) by Casey Schafer from the Whitman College gives a more complete explanation on linear algebra concepts and their applications in neural networks.

# References

[A friendly introduction to Recurrent Neural Networks](https://www.youtube.com/watch?v=UNmqTiOnRfg)

[Linear Algebra explained in the context of deep learning](https://towardsdatascience.com/linear-algebra-explained-in-the-context-of-deep-learning-8fcb8fca1494)

[Activation function](https://en.wikipedia.org/wiki/Activation_function)

[Neural Network Bias: Bias Neuron, Overfitting and Underfitting](https://missinglink.ai/guides/neural-network-concepts/neural-network-bias-bias-neuron-overfitting-underfitting/)

[How the backpropagation algorithm works](http://neuralnetworksanddeeplearning.com/chap2.html)

[Understanding the Mathematics behind Gradient Descent.](https://towardsdatascience.com/understanding-the-mathematics-behind-gradient-descent-dde5dc9be06e)

[What is backpropagation really doing? \| Deep learning, chapter 3](https://www.youtube.com/watch?v=Ilg3gGewQ5U)

[Backpropagation](https://brilliant.org/wiki/backpropagation/)

[The Neural Network, its Techniques and Applications](https://www.whitman.edu/Documents/Academics/Mathematics/2016/Schafer.pdf)
