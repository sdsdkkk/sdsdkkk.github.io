---
layout: content
title: "From Raw and Central Moments to Standardized Moment"
date: 2020-05-21 00:00:00
description: How raw and central moments are used to compose the standardized moment
---

This article serves as a note for me to compile everything I gathered from various sources about statistical moments while trying to make sense of the mathematical relations between each differing explanations and interpretations I found.  To be honest, learning about moments is a bit confusing for me since different sources seem to describe it somewhat differently.

In mathematics, [moment](https://en.wikipedia.org/wiki/Moment_(mathematics)) is used to represent density and the distribution of mass. Statistics is one of the contexts where we can apply moment, especially on distributions as the mass can be described in terms of moments.

I'm using [this video](https://www.youtube.com/watch?v=fv5QB3eK7jA) lecture by Sanjoy Mahajan as a reference for the following formula, but I'm using a different notation. The formula for the $$k$$-th moment of a distribution $$X$$ where $$x \in X$$ is as follows.

$$
E[X^k] = \Sigma_{i = 1}^n p_{i} x_{i}^k
$$

Another reference I'm using is [this document](http://geog.uoregon.edu/GeogR/topics/moments.pdf) from the geography department of the University of Oregon. The formula from the University of Oregon is centralizing the position of the center of mass to 0. The following is the formula they use for the moments with the mean of the observed sample $$\bar{X}$$.

$$
E[(X - \bar{X})^k] = \Sigma_{i = 1}^n (x_i - \bar{X})^k
$$

On this formula, the distribution is standardized to have its center of mass or its mean located at 0. They need to divide the result by $$ns^k$$ for third ($$k = 3$$) and fourth ($$k = 4$$) moments to get the skewness and kurtosis values. If we're looking at the the formula with Sanjoy Mahajan's lecture as a reference, $$n$$ should be the number of elements in $$X$$ assuming each element's probability is $$p = \frac{1}{n}$$ for $$x \in X$$. $$s$$ is the observed standard deviation of the finite sample.

$$
Skew[X] = \frac{E[(X - \bar{X})^3]}{ns^3} = \frac{\Sigma_{i = 1}^n (x_i - \bar{X})^k}{ns^3} \\
Kurt[X] = \frac{E[(X - \bar{X})^4]}{ns^4} = \frac{\Sigma_{i = 1}^n (x_i - \bar{X})^k}{ns^4}
$$

Wikipedia has another page on [standardized moment](https://en.wikipedia.org/wiki/Standardized_moment) which is supposedly based on the standardized setup where the first moment is $$\mu_{1} = 0$$ as the center of mass and is used as the base to calculate the rest of the moments. The formula stated there (as seen below) is using integration instead of summation, and aimed for continuous instead of discrete distributions.

$$
\mu_k = E[(X - \mu)^k] = \int_{- \infty}^{\infty} (x - \mu)^k P(x) dx
$$

But aside from the $$\mu_k$$, this also requires the $$\sigma$$ component which is the standard deviation of the distribution.

$$
\sigma^k = (\sqrt{E[(X - \mu)^2]})^k
$$

Both $$\mu_k$$ and $$\sigma^k$$ is used in the moment calculation as such.

| $$k$$-th Moment | Formula                                  |
|-----------------|------------------------------------------|
| 1st Moment      | $$\bar{\mu}_1 = \frac{\mu_1}{\sigma^1}$$ |
| 2nd Moment      | $$\bar{\mu}_2 = \frac{\mu_2}{\sigma^2}$$ |
| 3rd Moment      | $$\bar{\mu}_3 = \frac{\mu_3}{\sigma^3}$$ |
| 4th Moment      | $$\bar{\mu}_4 = \frac{\mu_4}{\sigma^4}$$ |

These equations are consistent with the formula used to calculate skewness and kurtosis we got from the University of Oregon before, so if we compare the formulas we should get the following formila for calculating the $$\mu_k$$ of a discrete distribution (which is consistent with Sanjoy Mahajan's formula).

$$
\mu_k = E[(X - \mu)^k] = \Sigma_{i = 1}^n (x - \mu)^k p_i
$$

Where $$p_i$$ is the probability of value $$x_i$$ appearing in the population.

On [this Wikipedia page](https://en.wikipedia.org/wiki/Moment_(mathematics)) we can see a table on the significance of moments. Turns out that moments are classified into raw, central, and standardized moments. The following table summarizes the Wikipedia entry, we can see what values are held by the raw, central, and standardized moments for a given distribution.

| $$k$$-th Moment | Raw     | Central  | Standardized   |
|-----------------|-------------------------------------|
| 1st Moment      | Mean    | 0        | 0              |
| 2nd Moment      | -       | Variance | 1              |
| 3rd Moment      | -       | -        | Skewness       |
| 4th Moment      | -       | -        | Kurtosis       |

# First Moment

As described in the moment table before, the first moment has a raw moment which is the mean of the distribution, along with a central and a standardized moment which is both 0. The first moment is used to indicate the center of mass of the distribution.

The mean is the first raw moment of the distribution.

$$
\mu = E[X]
$$

As for the central moment, we centralize the center of masss to the point 0. Therefore we get $$\mu_1$$ as follows.

$$
\mu_1 = E[(X - \mu)] = 0
$$

For the standardized moment, we can refer to the equation $$\bar{\mu}_1 = \frac{\mu_1}{\sigma^1}$$. Given that $$\mu_1 = E[(X - \mu)] = 0$$ and $$\sigma^1 = \sqrt{E[(X - \mu)^2]}$$ we can get the first standardized moment of the distribution as follows.

$$
\bar{\mu}_1 = \frac{\mu_1}{\sigma^1} = \frac{E[(X - \mu)]}{\sqrt{E[(X - \mu)^2]}} = \frac{0}{\sqrt{E[(X - \mu)^2]}} = 0
$$

# Second Moment

The second moment is used as the measurement of variability of the distribution around its mean. For this, the second moment's value is relative to the mean which is the center of mass. Because of that, we need to use central moment to calculate the second moment because we have to calculate the value relative to the mean.

$$
\mu_2 = E[(X - \mu)^2]
$$

Note that $$\sigma$$ which is the standard deviation is the square root of variance $$\sigma^2$$.

$$
\sigma^2 = (\sqrt{E[(X - \mu)^2]})^2 = E[(X - \mu)^2] 
$$

For the standardized moment, we can refer to the equation $$\bar{\mu}_2 = \frac{\mu_2}{\sigma^2}$$.

$$
\bar{\mu}_2 = \frac{\mu_2}{\sigma^2} = \frac{E[(X - \mu)^2]}{(\sqrt{E[(X - \mu)^2]})^2} = \frac{E[(X - \mu)^2]}{E[(X - \mu)^2]} = 1
$$

# Third Moment

The third moment is used as the measure of skewness or asymmetry in the shape of the probability distribution around its mean.

$$
\mu_3 = E[(X - \mu)^3]
$$

For the standardized moment, we can refer to the equation $$\bar{\mu}_3 = \frac{\mu_3}{\sigma^3}$$.

$$
\bar{\mu}_3 = \frac{\mu_3}{\sigma^3} = \frac{E[(X - \mu)^3]}{(\sqrt{E[(X - \mu)^2]})^3} = \frac{E[(X - \mu)^3]}{E[(X - \mu)^2]^{\frac{3}{2}}}
$$

With $$\mu$$ as the center of mass, the distribution is perfectly symmetrical when $$\bar{\mu}_3 = 0$$. It's skewed to the right when $$\bar{\mu}_3 < 0$$, as it has longer tail on the left side of the distribution which brings down the value to negative. If $$\bar{\mu}_3 > 0$$, the distribution is skewed to the left as it has longer tail to the right side of the mean which raises the value to positive.

# Fourth Moment

The fourth moment is used to measure the kurtosis, which is the measurement of the shape of the peak and tails of the probability distribution.

$$
\mu_4 = E[(X - \mu)^4]
$$

For the standardized moment, we can refer to the equation $$\bar{\mu}_4 = \frac{\mu_4}{\sigma^4}$$.

$$
\bar{\mu}_4 = \frac{\mu_4}{\sigma^4} = \frac{E[(X - \mu)^4]}{(\sqrt{E[(X - \mu)^2]})^4} = \frac{E[(X - \mu)^4]}{E[(X - \mu)^2]^2}
$$

Higher value on $$\bar{\mu}_4$$ means that the peak is more significantly higher than the tail, while lower value on $$\bar{\mu}_4$$ means that the peak is relatively flat to the tail. This means that for higher kurtosis value, the distribution has a lot of infrequent extreme deviations from the mean so the probability density in the tail is significantly smaller than the probability density around the mean.

# The Shape of the Distribution

The four moments are usually used to estimate the overall shape of the probability distribution. [This article](https://brownmath.com/stat/shape.htm) by Stan Brown has some good examples of pictures showing the shapes of the distribution to describe skewness and kurtosis values.

[This page](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35b.htm) from Engineering Statistics Handbook by NIST also provides some sample distribution shapes from random data with their skewness and kurtosis values.

Probability distributions such as hypergeometric, binomial, and Poisson distributions each has their own formula to calculate the mean, variance, skewness, and kurtosis of the distribution derived from these moment formulas. It's out of the coverage of this article though.

# References

[Moment (mathematics)](https://en.wikipedia.org/wiki/Moment_(mathematics))

[Moments of Distributions](https://www.youtube.com/watch?v=fv5QB3eK7jA)

[Statistical Moments (and the Shape of Distributions)](http://geog.uoregon.edu/GeogR/topics/moments.pdf)

[Standardized moment](https://en.wikipedia.org/wiki/Standardized_moment)

[Measures of Shape: Skewness and Kurtosis](https://brownmath.com/stat/shape.htm)

[1.3.5.11. Measures of Skewness and Kurtosis](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35b.htm)
