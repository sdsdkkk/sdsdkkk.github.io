---
layout: post
title: "Bayesian Statistics and Naive Bayes Classifiers"
date: 2020-09-20 00:00:00
description: The basic concept of Bayesian statistics and a quick look into some classifiers
tags: Statistics
---

[Bayesian statistics](https://en.wikipedia.org/wiki/Bayesian_statistics) is an approach in statistics where probability is interpreted as a degree in belief in an event. The probability of event $$A$$ happening given $$B$$ is true can be defined as follows.

$$
P(A \lvert B) = \frac{P(B \lvert A) P(A)}{P(B)}
$$

Where the probability is a degree of belief from the observation. Suppose we define event $$A$$ as the event where a traffic accident happening at a highway, and event $$B$$ as the event where it's raining in the area. Therefore, $$P(A)$$ is the probability of the traffic accident happening at the highway and $$P(B)$$ is the probability of rainfall in the area. Thus, $$P(B \lvert A)$$ is the probability of rainfall whenever an accident happens based on our belief (which is based on our observations). $$P(A \lvert B)$$ can then be calculated by plugging the numbers into the above formula.

Let's suppose we're observing the number of accidents in the highway when the area is having a drought and no rainfall has been observed throughout the observation period. We don't have any rainfall in the observation, but setting $$P(B) = 0$$ would imply that in our belief it's absolutely impossible for the area to have rainfall and will break the equation since calculating $$P(A \lvert B)$$ will involve division by zero.

In Bayesian statistics it's advised to avoid setting the probability of an event to zero, we can set it to a very small value instead when the event isn't observed during the observation and there's a very small probability that it might happen some time in the future.

The concepts of Bayesian statistics are applied in [naive Bayes classifiers](https://en.wikipedia.org/wiki/Naive_Bayes_classifier) to determine which events are more likely to happen given a data input, according to the beliefs built up from the past observation data processed by the classifiers for each possible class.

# Gaussian Naive Bayes

The following is the formula to calculate the probability of an event with the value for the feature $$x = v$$ belonging to a certain class $$C_k$$ in the list of classes $$C$$.

$$
p(x = v \lvert C_k) = \frac{1}{\sqrt{2 \pi \sigma_{k}^{2}}} e^{- \frac{(v - \mu_k)^2}{2 \sigma_{k}^{2}}}
$$

Gaussian naive Bayes classifier basically calculates the probability of the event belonging to the classes $$C_k \in C$$ given that the feature $$x$$ to have a certain value $$v$$ assuming the distribution of values in $$x$$ for each $$C_k$$.

For each class $$C_k$$, the distribution of the values for the feature $$x$$ is assumed as a Gaussian distribution described by the population mean $$\mu_k$$ and the standard deviation $$\sigma_k$$ which translates to the variance $$\sigma_{k}^2$$. The values of $$v$$, $$\mu_k$$, and $$\sigma_{k}^2$$ can be plugged into the formula to get the value of the probability $$p(x = v \lvert C_k)$$.

Since we're calculating the probability $$p(x = v \lvert C_k)$$ for each $$C_k \in C$$, we're going to have the probability $$P_k = p(x = v \lvert C_k)$$ and $$P_k \in P$$ where $$P$$ is the set of probabilities of $$x = v$$ for each element $$C_k \in C$$.

The probabilities $$P_k \in P$$ that we have calculated are basically the likelihood of the event belonging to the class $$C_k$$. We can simply classify the event as an occurence of $$C_k$$ which has the highest probability $$P_k$$ of having $$x = v$$.

Notice that the probability formula for the Gaussian naive Bayes is similar with the probability density function of the [Gaussian distribution](https://en.wikipedia.org/wiki/Normal_distribution) which is as follows.

$$
P(X = x) = \frac{1}{\sigma \sqrt{2 \pi}} e^{- \frac{1}{2} (\frac{x - \mu}{\sigma})^2}
$$

The formula can be transformed into the exact same form used in the Gaussian naive Bayes before.

$$
P(X = x) = \frac{1}{\sigma \sqrt{2 \pi}} e^{- \frac{1}{2} (\frac{x - \mu}{\sigma})^2} \\
P(X = x) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{- \frac{1}{2} (\frac{(x - \mu)^2}{\sigma^{2}})} \\
P(X = x) = \frac{1}{\sqrt{2 \pi \sigma^2}} e^{- \frac{(x - \mu)^2}{2 \sigma^{2}}}
$$

A sample usage of the Gaussian naive Bayes classifier implemented in scikit-learn can be seen in [this article](https://iq.opengenus.org/gaussian-naive-bayes/).

# Multinomial Naive Bayes

With a multinomial model, we assume a feature vector $$x = (x_1, x_2, x_3, ... , x_n)$$ as a histogram with $$x_i$$ is the number of times the event $$i$$ is observed. Each class $$k$$ also has a vector of probabilities $$p_{ki}$$ of the event $$i$$ happening for class $$C_k$$, which is denoted as $$p_k = (p_{k1}, p_{k2}, p_{k3}, ... , p_{kn})$$.

The following is the formula to calculate the probability $$p(x \lvert C_k)$$, which is the probability of feature vector $$x$$ belonging to the class $$C_k$$ with a known probability vector $$p_k$$.

$$
p(x \lvert C_k) = \frac{(\sum_i x_i)!}{\prod_i x_{i}!} \prod_i p_{ki}^{x_i}
$$

Similar to the case with Gaussian naive bayes before, from here we have the probability $$P_k = p(x \lvert C_k)$$ of feture vector $$x$$ belonging to the class $$C_k$$. After calculating the probability $$P_k$$ of $$x$$ belonging to the class $$C_k \in C$$, we're going to have the values $$P_k \in P$$ for the probability of $$x$$ belonging to $$C_k$$. From here, we can pick the class $$C_k$$ which has the highest $$P_k$$ value as the most likely class for the feature vector $$X$$ to belong to.

Just as Gaussian naive Bayes classifier before, the multinomial naive Bayes classifier is based on the probability mass function of the [multinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution).

$$
P(X) = \frac{n!}{x_{1}! x_{2}! x_{3}! ... x_{k}!} p_{1}^{x_1} p_{2}^{x_2} p_{3}^{x_3} ... p_{k}^{x_k}
$$

Which can be expressed as follows using the product notation.

$$
P(X) = \frac{n!}{\prod_i x_{i}!} \prod_i p_{i}^{x_i}
$$

Multinomial naive Bayes is commonly used for text classification. For a sample dataset with a lot of words, however, each word found in the dataset will become a feature in the feature vector $$x$$ and the feature vector has a high probability of being a [sparse vector](https://en.wikipedia.org/wiki/Sparse_matrix).

# Bernoulli Naive Bayes

Bernoulli naive Bayes is similar to multinomial naive Bayes in the sense that we're calculating the probability of a feature vector $$x = (x_1, x_2, x_3, ..., x_n)$$ belonging to the class. The difference is that for Bernoulli naive Bayes, each feature $$x_i$$ is assumed to be binary-valued which is either 1 or 0. Each class $$k$$ also has a vector of probabilities $$p_k = (p_{k1}, p_{k2}, p_{k3}, ..., p_{kn})$$ where $$p_{ki}$$ is the probability of $$x_i$$ having the value of 1.

$$
p(x \lvert C_k) = \prod_{i = 1}^{n} p_{ki}^{x_i} (1 - p_{ki})^{(1 - x_i)}
$$

Just like the previous naive Bayes classifiers we've covered, we can plug in the numbers into the above formula and find the class with the highest value of $$p(x \lvert C_k)$$ as the most likely class for the feature vector $$x$$ to belong to. The Bernoulli naive Bayes classifier is also based on a distribution whose namesake is taken as a reference of the classifier.

The following is the probability mass function formula for the [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution).

$$
P(X = x) = p^x (1 - p)^{(1 - x)}
$$

Where $$p$$ is the probability of $$X$$ having the value of $$x$$, where $$X \in \{0, 1\}$$. Suppose we have the probability of $$X = 1$$ defined as $$p = 0.2$$, if we plug in the numbers into the formula to find $$P(X = 1)$$ and $$P(X = 0)$$ we're going to get the result as follows.

$$
P(X = 1) = (0.2)^1 (1 - 0.2)^{(1 - 1)} = (0.2)^1 (0.8)^0 = (0.2)(1) = 0.2 \\
P(X = 0) = (0.2)^0 (1 - 0.2)^{(1 - 0)} = (0.2)^0 (0.8)^1 = (1)(0.8) = 0.8
$$

We can see that the Bernoulli naive Bayes formula is a bit different if compared to the Bernoulli distribution probability mass function, a bit different from the Gaussian and multinomial naive Bayes formulas in the previous sections which are exactly the same with the distributions their formula is derived from. This is because in the Bernoulli naive Bayes classifier, we're processing a feature vector which contains multiple Bernoulli random variables which are assumed to be independent from each other.

Since we're assuming all of the random events in the feature vector $$x = (x_1, x_2, x_3, ..., x_n)$$ to be independent from one another, the overall probability of the random events in the feature vector can be calculated as the product of the probabilities of each random event happening.

$$
P(x) = P(X = x_1) P(X = x_2) P(X = x_3) ... P(X = x_n) \\
P(x) = \prod_i P(X = x_i)
$$

[This article](https://iq.opengenus.org/bernoulli-naive-bayes/) shows an example of Bernoulli naive Bayes application and a sample use of scikit-learn for Bernoulli naive Bayes classification. [This other article](https://mattshomepage.com/articles/2016/Jun/07/bernoulli_nb/) by Matt Johnson digs deeper into the math of Bernoulli naive Bayes classifier and also gives us a sample implementation of the classifier.

# References

[Bayesian statistics](https://en.wikipedia.org/wiki/Bayesian_statistics)

[Naive Bayes classifier](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)

[Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution)

[Gaussian Naive Bayes](https://iq.opengenus.org/gaussian-naive-bayes/)

[Mutinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution)

[Sparse matrix](https://en.wikipedia.org/wiki/Sparse_matrix)

[Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution)

[Bernoulli Naive Bayes](https://iq.opengenus.org/bernoulli-naive-bayes/)

[Bernoulli Naive Bayes Classifier](https://mattshomepage.com/articles/2016/Jun/07/bernoulli_nb/)
