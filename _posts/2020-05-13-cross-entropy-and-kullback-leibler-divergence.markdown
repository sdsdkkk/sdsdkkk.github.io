---
layout: content
title: "Cross-Entropy and Kullback-Leibler Divergence"
date: 2020-05-13 00:00:00
description: Measuring the distance between prediction and reality in terms of encoding efficiency
---

In the context of information theory, cross-entropy is used to calculate the expected length of code representation for a message given we're assuming a probability distribution $$q$$ of the message's content which is different from the actual distribution $$p$$. Cross-entropy is formulated as follows.

$$
H(P, Q) = - \sum_{i=1}^n p_{i} log_{b} q_{i}
$$

The formula is derived from the [Shannon entropy](/2020/04/shannon-entropy-and-uncertainty-of-information.html) formula which only deals with one probability distribution $$p$$ in the calculation.

$$
H(P) = - \sum_{i=1}^n p_{i} log_{b} p_{i}
$$

Where the Shannon entropy formula assumes the optimal size of bit representation per message, the cross-entropy formula assumes the suboptimal case. The difference of the average number of symbols used to represent the message between the optimal and the real case is denoted as [Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence) or KL divergence, which is formulated as follows.

$$
D_{KL}(P || Q) = H(P, Q) - H(Q)
$$

Cross-entropy is commonly used as a loss function in machine learning. The base of the log $$b$$ can be set to any number we want to use, but it's usually set to 2 and in some cases might be set to 10 or $$e$$.

# Inexact Probability Distribution Transmission Encoding

Shannon entropy can be used to determine the optimal number of symbols per transmitted message in the context of communication. As the base $$b$$ used in the formula is commonly set to 2, the expected symbols are in bits. Suppose we have a string of message that may contain the characters $$X = \{A, C, G, T\}$$ in the string with the estimated probability for each character appearing in the message $$P = \{\frac{1}{4}, \frac{1}{4}, \frac{1}{4}, \frac{1}{4}\}$$.

With the Shannon entropy equation, we can calculate the estimated average amount of bits required to represent each character in the message as follows.

$$
H(P) = - \sum_{i=1}^4 p_{i} log_{2} p_{i}
$$

Since all of the characters have the same probability of appearing so we can calculate it as such.

$$
H(P) = - 4 \times (\frac{1}{4} log_{2} \frac{1}{4}) = - 4 \times (- \frac{2}{4} log_{2} 2) = - 4 \times (- \frac{2}{4}) = 2
$$

Given each character compose exactly 25% of the characters appearing in the message, the most optimal number of bits we can use to represent one character in the message is 2 bits per character.

Suppose we have a 12-character DNA sequence string $$AGTCTTAGACTG$$. The following is the number of times each characters appear in the string.

Character | Occurences |
----------|------------|
$$A$$     | 3          |
$$C$$     | 2          |
$$G$$     | 3          |
$$T$$     | 4          |

The actual distribution of the occurences for $$X = \{A, C, G, T\}$$ follows the probability distribution $$P = \{\frac{3}{12}, \frac{2}{12}, \frac{3}{12}, \frac{4}{12}\}$$. In this case, the most optimal expected number of bits used to represent each character can be estimated as follows.

$$
H(P) = - \sum_{i=1}^4 p_{i} log_{2} p_{i} \\
H(P) = - (\frac{3}{12} log_{2} \frac{3}{12} + \frac{2}{12} log_{2} \frac{2}{12} + \frac{3}{12} log_{2} \frac{3}{12} + \frac{4}{12} log_{2} \frac{4}{12}) \\
H(P) = - (\frac{3}{12} (-2) + \frac{2}{12} (-2.585) + \frac{3}{12} (-2) + \frac{4}{12} (-1.585)) \\
H(P) = - (- 0.5 - 0.431 - 0.5 - 0.528) \\
H(P) = 1.959
$$

So the estimated optimal number of bits to represent the DNA sequence string is 1.959 bits per character.

Suppose that we don't know the actual probability distribution $$P$$ and we're assuming it to be $$Q = \{\frac{1}{4}, \frac{1}{4}, \frac{1}{4}, \frac{1}{4}\}$$. Now we can calculate the cross-entropy as follows.

$$
H(P, Q) = - \sum_{i=1}^4 p_{i} log_{2} q_{i} \\
H(P, Q) = - (\frac{3}{12} log_{2} \frac{1}{4} + \frac{2}{12} log_{2} \frac{1}{4} + \frac{3}{12} log_{2} \frac{1}{4} + \frac{4}{12} log_{2} \frac{1}{4}) \\
H(P, Q) = - (\frac{3}{12} (-2) + \frac{2}{12} (-2) + \frac{3}{12} (-2) + \frac{4}{12} (-2)) \\
H(P, Q) = - (- \frac{6}{12} - \frac{4}{12} - \frac{6}{12} - \frac{8}{12}) \\
H(P, Q) = - (- \frac{24}{12}) \\
H(P, Q) = 2
$$

The cross-entropy $$H(P, Q) = 2$$, meaning that we're using on average 2 bits to represent the characters with the actual distribution $$P$$ as we're assuming the distribution $$Q$$ when encoding the message.

The extra number of bits we're using to encode the message on average when assuming expected probability distribution $$Q$$ when the actual distribution of $$P$$ is as follows.

$$
D_{KL}(P || Q) = H(P, Q) - H(Q) = 2 - 1.959 = 0.041
$$

We're using about 0.041 bits more than the optimal number of bits on average when encoding the message during the transmission. Though in this example the difference isn't that much since we're only using four possible characters and the probability of each character appearing isn't that much different in the real distribution compared to the assumed distribution.

# Loss Function

[Loss function](https://machinelearningmastery.com/loss-and-loss-functions-for-training-deep-learning-neural-networks/) is a function to calculate loss, which is the error made by a trained model. The loss function is used when optimizing a machine learning model as a function to measure how far off the model's prediction is, and tuning the model to minimize the loss.

There are [several loss functions](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/) to choose from, some might be more well-suited than others in their specific cases. In general, the functions we can use as a loss function are those that can be used to measure distance between a model's prediction compared to the real data.

Since we're trying to measure the distance between the prediction and the reality, we can use cross-entropy and KL divergence to find the difference and therefore we can use them as a loss function. That application isn't something that I've explored at this point, so it's something I'm saving for later.

# References

[Shannon Entropy and Uncertainty of Information](/2020/04/shannon-entropy-and-uncertainty-of-information.html)

[Kullback-Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)

[Loss and Loss Functions for Training Deep Learning Neural Networks](https://machinelearningmastery.com/loss-and-loss-functions-for-training-deep-learning-neural-networks/)

[How to Choose Loss Functions When Training Deep Learning Neural Networks](https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/)
