---
layout: post
title: "Gamma Distribution, Poisson Process, and Gamma Function"
date: 2020-08-02 00:00:00
description: Gamma distribution's Poisson process modeling and a bit of gamma function
tags: Statistics
---

Gamma distribution is used to model the waiting time of a Poisson process, similar to [exponential distribution](/2020/07/exponential-distribution.html). According to [this article](https://towardsdatascience.com/gamma-distribution-intuition-derivation-and-examples-55f407423840) by Aerin Kim, the difference is that the exponential distribution is used to model the wait time until the next event happens, while the gamma distribution is used to model the wait time until the $$k$$-th next event happens.

# Probability Density Function

The waiting time until the $$k$$-th Poisson event can be calculated using the following formula.

$$
P(x) = \frac{\lambda(\lambda x)^{k - 1}}{(k - 1)!} e^{- \lambda x}
$$

Note that I'm referencing the definition in the gamma distribution materials on [UCLA's wiki](http://wiki.stat.ucla.edu/socr/index.php/AP_Statistics_Curriculum_2007_Gamma) from their statistics department.

$$(k - 1)!$$ can be denoted as the [gamma function](https://en.wikipedia.org/wiki/Gamma_function) for whole real numbers.

$$
\Gamma(k) = (k - 1)!
$$

We can see the similarity of the waiting time function for $$k$$-th Poisson event above with the exponential distribution's probability density function below.

$$
f(x) = \lambda e^{- \lambda x}
$$

The similarity is due to how they functions are related to each other as the exponential distribution is used to estimate time to wait until the first occurence of an event in a [Poisson process](/2020/05/deriving-poisson-probability-function-from-binomial-distribution.html) with the rate $$\lambda$$.

Given that $$k = 1$$, we're going to get the exponential distribution's probability density function as shown below.

$$
f(x) = \frac{\lambda(\lambda x)^{k - 1}}{(k - 1)!} e^{- \lambda x} \\
f(x) = \frac{\lambda(\lambda x)^{1 - 1}}{(1 - 1)!} e^{- \lambda x} \\
f(x) = \frac{\lambda(1)}{(0)!} e^{- \lambda x} \\
f(x) = \lambda e^{- \lambda x}
$$

The UCLA notes describes the gamma distribution probability density function with $$\theta$$ where $$\theta = \frac{1}{\lambda}$$.

$$
f(x) = \frac{x^{k - 1} e^{- \frac{x}{\theta}}}{\Gamma(k) \theta^{k}}
$$

Which is equivalent to the one described on [Wikipedia](https://en.wikipedia.org/wiki/Gamma_distribution) but with $$\alpha = k$$ and $$\beta = \lambda$$.

$$
f(x) = \frac{\beta^{\alpha} x^{\alpha - 1} e^{- \beta x}}{\Gamma(\alpha)} = \frac{\lambda^{k} x^{k - 1} e^{- \lambda x}}{\Gamma(k)}
$$

The gamma distribution probability density function is basically the probability function of waiting time to the $$k$$-th Poisson event shown before, but simplified.

$$
P(x) = \frac{\lambda(\lambda x)^{k - 1}}{(k - 1)!} e^{- \lambda x} \\
P(x) = \frac{\lambda (\lambda)^{k - 1} x^{k - 1}}{(k - 1)!} e^{- \lambda x} \\
P(x) = \frac{\lambda^{k} x^{k - 1}}{\Gamma(k)} e^{- \lambda x} \\
$$

Insert $$k = 1$$ and we're going to get the exponential distribution's probability density function as before. Therefore, we can say that the exponential distribution is a special case of the gamma distribution.

# Cumulative Distribution Function

The cumulative distribution function of the gamma distribution can be defined as follows.

$$
F(x) = \frac{\gamma(k, \lambda x)}{\Gamma(k)}
$$

The following is the generalized definition of the gamma function by [Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler) for $$x > 0$$.

$$
\gamma(k, x) = \int_{0}^{x} t^{k - 1} e^{-t} dt
$$

The generalized gamma function was introduced by Leonhard Euler in his effort to generalize the factorial to non-integer values. [This article](http://numbers.computation.free.fr/Constants/Miscellaneous/gammaFunction.html) might be a good reference in order to understand the gamma function and its significance better.

Now, let's take a look once again to the probability function of the waiting time for the $$k$$-th Poisson event we used to derive the probability density function for both gamma and exponential distributions before.

$$
f(x) = \frac{\lambda(\lambda x)^{k - 1}}{(k - 1)!} e^{- \lambda x}
$$

Note that the pattern in the gamma distribution probability density function is quite similar to the gamma function. We can rearrange it a bit to make it clearer.

$$
f(x) = \frac{\lambda}{\Gamma(k)} (\lambda x)^{k - 1} e^{- \lambda x}
$$

From here, it's quite visible how the cumulative distribution function with the $$\gamma(k, \lambda x)$$ gamma function definition comes from the integration of the probability density function. The thing is that I've tried to integrate it by myself but it the result seems to be somewhat off, but I haven't been able to find any references that can be used as a comparison with my integration result so I'm going to omit that from this note since mine seems to be wrong. I might write it separately later after I got it right.

I think I'll also need to explore more about the gamma function later, as I've just got introduced to the concept while exploring the gamma distribution.

# References

[Exponential Distribution](/2020/07/exponential-distribution.html)

[Gamma Distribution — Intuition, Derivation, and Examples](https://towardsdatascience.com/gamma-distribution-intuition-derivation-and-examples-55f407423840)

[AP Statistics Curriculum 2007 Gamma](http://wiki.stat.ucla.edu/socr/index.php/AP_Statistics_Curriculum_2007_Gamma)

[Gamma function](https://en.wikipedia.org/wiki/Gamma_function)

[Deriving Poisson Probability Function from Binomial Distribution](/2020/05/deriving-poisson-probability-function-from-binomial-distribution.html)

[Gamma distribution](https://en.wikipedia.org/wiki/Gamma_distribution)

[Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler)

[Introduction to the Gamma Function](http://numbers.computation.free.fr/Constants/Miscellaneous/gammaFunction.html)
