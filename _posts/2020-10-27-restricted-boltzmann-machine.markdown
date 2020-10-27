---
layout: content
title: "Restricted Boltzmann Machine"
date: 2020-10-27 00:00:00
description: An overview of the restricted Boltzmann machine
---

[Boltzmann machine](https://en.wikipedia.org/wiki/Boltzmann_machine) is a type of neural network which is inspired by the work of [Ludwig Boltzmann](https://en.wikipedia.org/wiki/Ludwig_Boltzmann) in the field of [statistical mechanics](https://en.wikipedia.org/wiki/Statistical_mechanics).

We're specifically looking at a version of Boltzmann machine called the [restricted Boltzmann machine](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine) in this article. A standard restricted Boltzmann machine consists of visible and hidden units. The visible and hidden units has the binary value of 0 or 1, and a matrix $$W = [w_{i, j}]$$ with the size $$m \times n$$ containing the weights of the connection between each visible unit $$v_i$$ and each hidden unit $$h_j$$.

The following diagram shows the general structure of a restricted Boltzmann machine.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,4.25) node {Visible Layer};
    \draw (5.5,4.25) node {Hidden Layer};

    \draw (0,0) rectangle (3,4);
    \draw (1.5,3.25) circle (0.2in);
    \draw (1.5,2) circle (0.2in);
    \draw (1.5,0.75) circle (0.2in);

    \draw (4,0) rectangle (7,4);
    \draw (5.5,3.25) circle (0.2in);
    \draw (5.5,2) circle (0.2in);
    \draw (5.5,0.75) circle (0.2in);

    \draw [-] (2,3.25) -- (5,3.25);
    \draw [-] (2,3.25) -- (5,2);
    \draw [-] (2,3.25) -- (5,0.75);

    \draw [-] (2,2) -- (5,3.25);
    \draw [-] (2,2) -- (5,2);
    \draw [-] (2,2) -- (5,0.75);

    \draw [-] (2,0.75) -- (5,3.25);
    \draw [-] (2,0.75) -- (5,2);
    \draw [-] (2,0.75) -- (5,0.75);
  \end{tikzpicture}
</script>

[This video](https://www.youtube.com/watch?v=puux7KZQfsE) shows an animated explanation of the restricted Boltzmann machine.

# Weight Matrix and Energy Function

Suppose the visible units are $$V \in \{ v_1, v_2, v_3 \}$$ and the hidden units are $$H \in \{ h_1, h_2, h_3 \}$$. Each weight value $$w_{i,j}$$ represents the weight of the relation between visible unit $$v_i$$ and hidden unit $$h_j$$. Therefore, we can define the weight matrix $$W$$ for the restricted Boltzmann machine above as follows.

$$
W = \left[
  \begin{matrix}
  w_{1,1} & w_{1,2} & w_{1,3} \\
  w_{2,1} & w_{2,2} & w_{2,3} \\
  w_{3,1} & w_{3,2} & w_{3,3}
  \end{matrix}
\right]
$$

Given that $$a_i$$ is the bias weight for the visible unit $$v_i$$ and $$b_j$$ is the bias weight for the hidden unit $$h_j$$, the total energy of the system can be calculated using the following formula.

$$
E(v, h) = - \sum_{i} a_i v_i - \sum_{j} b_j h_j - \sum_{i} \sum_{j} v_i w_{i,j} h_j
$$

Or if we perform the computation using the matrix form, we can use the following formula.

$$
E(v, h) = - a^{T} v - b^{T} h - v^{T} W h
$$

The probability density function for the system over both the visible and hidden layers can be defined as follows.

$$
P(v, h) = \frac{1}{Z} e^{- E(v, h)}
$$

The probability density function for the visible layer of the system can be defined as follows.

$$
P(v) = \frac{1}{Z} \sum_h e^{- E(v, h)}
$$

Where $$Z$$ is a partition function and defined as follows.

$$
Z = \sum_{v, h} e^{- E(v, h)}
$$

# Model Building

Looking at the structure of restricted Boltzmann machine, we can see that it's a neural network with only two layers. The difference between a regular neural network, the network doesn't have any input or output layers.

[This video](https://www.youtube.com/watch?v=Fkw0_aAtwIw) by Luis Serrano gives us a more detailed explanation on how a restricted Boltzmann machine works. The nodes in the visible layer represent the events we can observe in our dataset, while the hidden layers represent the hidden variable that we can't se in our dataset that might be affecting the observable events we're analyzing.

When training the model, we need to define the nodes in the visible layer according to the observed data. The number of nodes in the hidden layer is defined arbitrarily, we can try to test various numbers of hidden units and see the number of hidden units which yields the best result in the model.

Restricted Boltzmann machines are commonly used to perform dimensionality reduction. In this case as mentioned in [this article](https://medium.com/edureka/restricted-boltzmann-machine-tutorial-991ae688c154) by Sayantini Deb, we want to reduce the number of dimensions for data analysis where the original number of dimensions are the number of visible units. Since we're expecting to reduce the dimension for analysis, we set up the hidden units to be fewer than the visible units and train the model to fit the observed data. The hidden units can then be used as variables for further analysis.

The end goal of the model is that given a set of events according to the nodes in the visible layer, we can trace which hidden units are more likely to be involved in the observed events and what other events in the visible layer are likely to happen based on the connection of the hidden units with the rest of the visible units. In the training phase, the weights and biases of the nodes are increased and decreased to adjust the model to represent the training data.

# Gibbs Sampling

As explained in [the video](https://www.youtube.com/watch?v=Fkw0_aAtwIw) by Luis Serrano, when we have too many connections between the nodes in the visible and hidden layers, we're going to face a problem since to calculate the partition function we need to iterate the calculation of the energy function for every visible unit $$v_i$$ and hidden unit $$h_j$$ pair and there will be several connections that includes the visible layers we're expecting whose probability values can be optimized separately. This problem can be avoided by using [Gibbs sampling](http://www.mit.edu/~ilkery/papers/GibbsSampling.pdf).

Gibbs sampling is a [Markov chain Monte Carlo](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) (MCMC) method to obtain a sequence of observations which are approximated from a specified multivariate distribution, as explained in [the Wikipedia page](https://en.wikipedia.org/wiki/Gibbs_sampling).

By using Gibbs sampling, we can sample only one scenario that matches all of the visible events in the records in the data points that includes a hidden unit in the scenario and focusing on optimizing for the probability values for the scenario according to our dataset. After that, we can perform a random walk for a few steps to another scenario and adjust the weights to reduce the probability of the scenario. This way, we don't need to compute the weights for irrelevant connections to make the computation process more efficient.

[This video](https://www.youtube.com/watch?v=tre4zz7pnlE) provides a short explanation and a demonstration of Gibbs sampling.

# Extra Notes

[This video](https://www.youtube.com/watch?v=8EaVZbmAnV0) from the Cognitive Class YouTube channel shows a demonstration on how to utilize restricted Boltzmann machines for a recommendation system implementation.

# References

[Boltzmann machine](https://en.wikipedia.org/wiki/Boltzmann_machine)

[Ludwig Boltzmann](https://en.wikipedia.org/wiki/Ludwig_Boltzmann)

[Statistical mechanics](https://en.wikipedia.org/wiki/Statistical_mechanics)

[Restricted Boltzmann machine](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine)

[Restricted Boltzmann Machines - Ep. 6 (Deep Learning SIMPLIFIED)](https://www.youtube.com/watch?v=puux7KZQfsE)

[Restricted Boltzmann Machines - A friendly introduction](https://www.youtube.com/watch?v=Fkw0_aAtwIw)

[Restricted Boltzmann Machine Tutorial — A Beginner’s Guide To RBM](https://medium.com/edureka/restricted-boltzmann-machine-tutorial-991ae688c154)

[Bayesian Inference: Gibbs Sampling](http://www.mit.edu/~ilkery/papers/GibbsSampling.pdf)

[Markov chain Monte Carlo](https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo)

[Gibbs sampling](https://en.wikipedia.org/wiki/Gibbs_sampling)

[MCMC and the Gibbs Sampling Example](https://www.youtube.com/watch?v=tre4zz7pnlE)

[Deep Learning with Tensorflow - Recommendation System with a Restrictive Boltzmann Machine](https://www.youtube.com/watch?v=8EaVZbmAnV0)
