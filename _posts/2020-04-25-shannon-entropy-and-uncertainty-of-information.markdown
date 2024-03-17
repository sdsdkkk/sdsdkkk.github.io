---
layout: post
title: "Shannon Entropy and Uncertainty of Information"
date: 2020-04-25 00:00:00
description: An attempt to better understand Shannon entropy as a measurement of uncertainty
tags: ComputerScience InformationTheory
---

Shannon entropy is commonly used in malware analysis, and I actually started writing this article after an attempt to better understand Shannon entropy after reading [this paper](https://arxiv.org/pdf/1909.07227.pdf) by Duc-Ly Vu and team.

The following is the formula for [Shannon entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)), the measurements of entropy in information theory and also known as information entropy. Shannon defined the entropy $$H$$ after [Boltzmann's H-theorem](https://en.wikipedia.org/wiki/H-theorem).

$$
H(X) = - \sum_{i=1}^n P(x_{i}) log_{b} P(x_{i})
$$

Where $$x$$ is a discrete random variable and $$x \in X$$. Given that the distribution of elements in $$X$$ follows the probability distribution $$P$$, the total probability of $$X$$ is denoted as $$P(X)$$ and for $$x_{i} \in X$$ the probability is $$P(x_{i})$$.

$$b$$ is the base of the logarithm, for which the common value used in the context of Shannon entropy is $$b = 2$$ assuming the information is encoded using binary representation of 0 and 1.

# Demonstration

Suppose a coin flip where the result is either head or tail. Given $$X = \{ h, t \}$$ and $$x \in X$$, where $$P(h) = \frac{1}{2}$$ and $$P(t) = \frac{1}{2}$$. We can calculate the entropy $$H(X)$$ as follows, by plugging in the variables on the equation with what we know from the problem definition.

$$
H(X) = - \sum_{i=1}^2 P(x_{i}) log_{2} P(x_{i})
$$

Now we can expand the sigma into this.

$$
H(X) = - (P(h) log_{2} P(h) + P(t) log_{2} P(t))
$$

We already know the value of $$P(h)$$ and $$P(t)$$ from the problem definition.

$$
H(X) = - (\frac{1}{2} log_{2} \frac{1}{2} + \frac{1}{2} log_{2} \frac{1}{2})
$$

Based on the rule $$log_{b} \frac{1}{c} = - log_{b} c$$, we can proceed with the equation as follows.

$$
H(X) = - (\frac{1}{2}(- log_{2} 2) + \frac{1}{2}(- log_{2} 2))
$$

Since $$log_{b} b = 1$$, we can continue with this.

$$
H(X) = - (\frac{1}{2}(-1) + \frac{1}{2}(-1)) = - (- 2 (\frac{1}{2})) = 2 (\frac{1}{2}) = 1
$$

Therefore, $$H(X) = 1$$ for one coin flip with unknown result between head or tail.

# Further into the Concepts

I started by using Aerin Kim's explanation [here](https://towardsdatascience.com/the-intuition-behind-shannons-entropy-e74820fe9800) as I consider it a simpler and more intuitive reference to help me understand the context faster.

$$
H(X) = - \sum_{i=1}^n P(x_{i}) log_{b} P(x_{i}) \\
H(X) = \sum_{i=1}^n P(x_{i}) log_{b} \frac{1}{P(x_{i})}
$$

$$P(x)$$ is the probability of the occurence of event $$x$$, while $$log_{b} \frac{1}{P(x)}$$ is the amount of uncertainty according to Aerin Kim's article and the discussions of the article. This is a bit confusing for me so after reading a few more references and watching a few short lectures on Shannon entropy I decided to simply refer to Claude Shannon's [paper](http://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf) to help understand about the concepts of his entropy equation.

The thing about Shannon entropy equation which confused me is that it seems to be interpreted as different things by different people who provides explanation on it. For some it's a measurement of randomness, for some others it's an estimation of the number of bits required to represent a message in a payload for communication systems (which seems to be Shannon's initial idea). The paper written by Claude Shannon kicked off information theory and influenced many other fields which utilizes his ideas for different problems.

Suppose a possible string of characters to be sent as a message through a communication channel. The set of characters contains the letters A to Z. Without knowing what composition of the characters will be used for the string, we can estimate the number of bits we can use to represent each individual characters using logarithm with base 2 for 26 possible characters in the alphabet.

$$
log_{2} 26 = 4.7
$$

With this, some of the characters will be able to be represented with 4 bits but most of them will be represented with 5 bits if a balanced binary search tree is constructed from the set. The average bit string length for the character bit representations is 4.7 bits.

So to send a message containing up to 10 characters containing only the characters A to Z in it, given we don't know what would be the content inside the message, we need to ensure the communication system can handle up to $$4.7 \times 10 = 47$$ bits per message.

Assuming all characters has the same probability of $$\frac{1}{26}$$ to appear in the message, we can estimate the number of minimum bits to use in the message per character as follows.

$$
H(X) = - \sum_{i=1}^{26} P(x_{i}) log_{2} P(x_{i}) = - 26 \times (\frac{1}{26} log_{2} \frac{1}{26})
$$

So the number of minimum bits to use in the message per character is as below.

$$
H(X) = - 26 \times (- \frac{1}{26} log_{2} 26) = - 26 \times (- \frac{1}{26} \times 4.7) = 4.7
$$

Which is the same with our calculation of $$log_{2} 26 = 4.7$$ as before assuming all characters have equal probabilities of appearing in the message.

Let's suppose we have a 10-character message $$HAHAHAHAHA$$. If a random character is picked from the message, $$P(H) = \frac{1}{2}$$, $$P(A) = \frac{1}{2}$$, and for the rest of characters $$P = 0$$ as they don't appear at all in the message. With the knowledge that the message only contains the characters A and H, we can represent the message with less bits if we consider the characters $$x$$ in the message to be $$x \in \{A, H\}$$.

$$
log_{2} 2 = 1
$$

1 is the depth of the binary search tree we're going to have if we're building one to search only for A and H characters.

Plugged into the Shannon entropy equation, we're going to get the result as follows (similar with the head or tail coin flips in the demonstration).

$$
H(X) = - \sum_{i=1}^2 P(x_{i}) log_{2} P(x_{i}) = - 2 \times (\frac{1}{2} log_{2} \frac{1}{2}) = - 2 \times (- \frac{1}{2}) = 1
$$

The more information we know about what is contained in the message, the less bits we can use to represent the message as we have less uncertainty about what kind of message we're going to represent. As soon as we can ensure the possible characters to appear in the message from the whole alphabet to simply A and H, we can use fewer bits to represent it because we know exactly what we need to represent.

When all the communicating parties already know in advance that the message is going to be $$HAHAHAHAHA$$ instead of any other 10-character message containing the letters A and H, we can consider the whole $$HAHAHAHAHA$$ message as one piece of information and if we build a binary search tree for it we're going to have zero depth as there's nothing to search for since we only have one element in the set and $$P(X) = \{1\}$$.

$$
log_{2} 1 = 0
$$

Therefore, this is what we're going to get from the Shannon entropy equation.

$$
H(X) = - \sum_{i=1}^1 P(x_{i}) log_{2} P(x_{i}) = - (1 \times log_{2} \frac{1}{1}) = - (- 1 \times 0) = 0
$$

There's no uncertainty at all in a message that's already known to contain the string $$HAHAHAHAHA$$ so we don't need to send anything to make it known to the receiving party anymore since there's no new information for them to gain.

Assuming we're dealing with a message that's known to consist of 10 characters with five letter As, three letter Bs, and two letter Cs, we have $$x \in X$$ where $$X = \{A, B, C\}$$ and $$P(x_{i}) \in P(X)$$ where $$P(X) = \{\frac{1}{2}, \frac{3}{10}, \frac{1}{5}\}$$

We can plug them directly to the Shannon entropy equation.

$$
H(X) = - \sum_{i=1}^3 P(x_{i}) log_{2} P(x_{i})
$$

Now, since we have three elements in $$X$$ with different probabilities, we need to calculate them and sum the result instead of calculating just one and multiplying it by the number of elements in $$X$$ like before.

$$
H(X) = - (P(A) log_{2} P(A) + P(B) log_{2} P(B) + P(C) log_{2} P(C))
$$

So we're going to get this.

$$
H(X) = - (\frac{1}{2} log_{2} \frac{1}{2} + \frac{3}{10} log_{2} \frac{3}{10} + \frac{1}{5} log_{2} \frac{1}{5}) = - (- 0.5 - 0.521 - 0.464)
$$

Here's our result.

$$
H(X) = 0.5 + 0.521 + 0.464 = 1.485
$$

So we can use about 1.485 bits on average to represent each characters given that we already know the composition of the characters in the message but not their actual positions in it.

Basically, the higher $$H(X)$$ translates to the higher amount of bits expected to represent the information in the message due to the more possible information that can be contained in it. The less we know about the message, there's a higher number of possibilities that we have to anticipate when handling it. As we know more about the content of the message, we have less uncertainty in the message that we need to anticipate so we can use fewer bits to represent the information contained in the message based on what we already know about its content.

Therefore, Shannon entropy works by measuring the number of bits to represent a message. The higher the number of bits required to represent every possible unkown combination of the units of information within the message means that the less certainty we have regarding the content of the information. Thus, both the explanations which interpret Shannon entropy as a method for determining capacity by calculating the number of bits a communication system should be able to handle and the explanations which interpret Shannon entropy as a measure of uncertainty are correct.

# References

[A Convolutional Transformation Network for Malware Classification](https://arxiv.org/pdf/1909.07227.pdf)

[Entropy (information theory)](https://en.wikipedia.org/wiki/Entropy_(information_theory))

[H-theorem](https://en.wikipedia.org/wiki/H-theorem)

[A Mathematical Theory of Communication](http://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf)

[The intuition behind Shannon's Entropy](https://towardsdatascience.com/the-intuition-behind-shannons-entropy-e74820fe9800)
