---
layout: post
title: "Weibull Distribution"
date: 2020-07-14 00:00:00
description: Taking a first look into the Weibull distribution
tags: Statistics
---

We're going to take a look into the [Weibull distribution](https://en.wikipedia.org/wiki/Weibull_distribution). As explained in [this slide](http://www.thphys.nuim.ie/Notes/EE304/Notes/LEC10/ExpWeibull_handout.pdf) from Maynooth University, the Weibull distribution is commonly used to model the time to failure of a system due to deterioration over time.

I haven't really encountered any cases where Weibull distribution should be used before, but I've heard about this distribution. There are some concepts in the distribution I'm still unfamiliar with, so I'm collecting references while trying to read more resources as I'm writing this notes.

# Probability Density Function

The probability density function of the Weibull distribution is defined as follows.

$$
f(x) = \frac{k}{\lambda}(\frac{x}{\lambda})^{k - 1} e^{-(\frac{x}{\lambda})^k}
$$

$$k$$ is the shape parameter, while $$\lambda$$ is the scale parameter of the distribution. This [wiki article](http://reliawiki.org/index.php/The_Weibull_Distribution) explains the shape and scale parameters, but using different symbols in the equation.

The shape parameter $$k$$ describes where the highest point of the probability density is located, while the scale parameter $$\lambda$$ describes how stretched the probability density curve is.

[This reference](https://www.fpl.fs.fed.us/documnts/fplgtr/fpl_gtr264.pdf) from the US Department of Agriculture explains how to estimate the parameters for a Weibull distribution.

# Cumulative Distribution Function

The following is the cumulative distribution function of the Weibull distribution.

$$
P(X < x) = F(x) = 1 - e^{-(\frac{x}{\lambda})^{k}}
$$

As the case with the [exponential distribution](/2020/07/exponential-distribution.html), the cumulative distribution function is acquired from the integration of the probability density function to calculate the area under the curve over a certain interval.

$$P(X < x)$$ is the probability for the system to fail before the time reaches point $$x$$. Therefore, we can define the probability of the system outlasts point $$x$$ in time as follows where $$P(X < x) + P(X > x) = 1$$.

$$
P(X < x) = 1 - e^{-(\frac{x}{\lambda})^{k}} \\
P(X < x) - 1 = -e^{-(\frac{x}{\lambda})^{k}} \\
P(X < x) - (P(X < x) + P(X > x)) = -e^{-(\frac{x}{\lambda})^{k}} \\
P(X < x) - P(X < x) - P(X > x) = -e^{-(\frac{x}{\lambda})^{k}} \\
-P(X > x) = -e^{-(\frac{x}{\lambda})^{k}} \\
P(X > x) = e^{-(\frac{x}{\lambda})^{k}}
$$

The function that gives the probability $$P(X > x)$$ is the reliability function of the distribution, usually denoted as $$R(x)$$ with $$F(x)$$ as the cumulative distribution function.

$$
R(x) = e^{-(\frac{x}{\lambda})^{k}} \\
R(x) = 1 - F(x)
$$

# Hazard Function

Weibull distribution has a hazard function that's defined as follows.

$$
h(x) = kx^{(k - 1)}
$$

And the cumulative hazard function is defined as follows.

$$
H(x) = x^k
$$

Simply by looking at the hazard function and the cumulative hazard function we can easily see that the cumulative hazard function is an integral of the hazard function.

$$
H(x) = \int h(x) dx \\
H(x) = \int kx^{(k - 1)} dx \\
H(x) = \frac{1}{k}(kx^{k}) \\
H(x) = x^k
$$

[This article](https://www.statisticshowto.com/hazard-function/) gives some explanation about the hazard function. [This article](https://rss.onlinelibrary.wiley.com/doi/pdf/10.1111/j.1740-9713.2018.01123.x) from Wiley's online library also have a some.

The hazard function describes the failure rate of the distribution.

# Extra Notes

There are also some other functions that can be derived from the Weibull distribution such as the survival function and the inverse survival function as described [here](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3668.htm).

# References

[Weibull distribution](https://en.wikipedia.org/wiki/Weibull_distribution)

[Exponential and Weibull Distributions](http://www.thphys.nuim.ie/Notes/EE304/Notes/LEC10/ExpWeibull_handout.pdf)

[The Weibull Distribution](http://reliawiki.org/index.php/The_Weibull_Distribution)

[Procedures for Estimation of Weibull Parameters](https://www.fpl.fs.fed.us/documnts/fplgtr/fpl_gtr264.pdf)

[Exponential Distribution](/2020/07/exponential-distribution.html)

[Hazard Function: Simple Definition](https://www.statisticshowto.com/hazard-function/)

[The Weibull distribution](https://rss.onlinelibrary.wiley.com/doi/pdf/10.1111/j.1740-9713.2018.01123.x)

[1.3.6.6.8. Weibull Distribution](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3668.htm)
