---
layout: post
title: "Exponential Distribution"
date: 2020-07-03 00:00:00
description: Understanding how the cumulative distribution function is constructed
tags: Statistics
---

According to [this note](https://onlinelibrary.wiley.com/doi/pdf/10.1002/9781119197096.app03) from the book [Advanced Analytical Models](https://www.amazon.com/Advanced-Analytical-Models-Applications-Accord/dp/047017921X) by Johnathan Mun, the exponential distribution is used to describe events recurring at random points in time while the Weibull distribution is used to describe time to failure in reliability studies. In this case, both distributions are commonly used for predicting the reliability of a system by measuring the time to failure.

[This slide](http://www.thphys.nuim.ie/Notes/EE304/Notes/LEC10/ExpWeibull_handout.pdf) from Maynooth University notes that the difference in the usage of exponential and Weibull distributions in estimating time to failure is that exponential distribution is used to estimate the time to failure from sudden events happening to the system while Weibull distribution is used to estimate the time to failure from slow deterioration of the system over time.

We're going to focus on the exponential distribution on this article.

# Probability Density Function

The probability density function of the exponential distribution is defined as follows, given $$x \geq 0$$ is the interval between events where $$\lambda > 0$$ is the rate of event happening per interval.

$$
f(x) = \lambda e^{-\lambda x}
$$

If $$x < 0$$, then $$f(x) = 0$$.

Exponential distribution is related to the Poisson distribution, as the Poisson distribution is used to estimate the number of occurences of an event during a certain interval while the exponential distribution is used to estimate the size of interval between occurences. [This article](https://openstax.org/books/introductory-business-statistics/pages/5-3-the-exponential-distribution) from OpenStax explains the relation between Poisson and exponential distributions.

Exponential distribution is considered memoryless, when used to model a failure of a component we consider the component's age to have no effect to the probability of the failure event happening.

The mean time to event happening $$\mu$$ is defined as follows.

$$
\mu = \frac{1}{\lambda}
$$

Since $$\mu = \frac{1}{\lambda}$$, we can also say that the following is true.

$$
\lambda = \frac{1}{\mu}
$$

That means if an event is happening in the rate of $$\lambda$$ times per hour, we can expect one event to happen every $$\mu = \frac{1}{\lambda}$$ hour.

# Cumulative Distribution Function

The cumulative distribution function of the exponential distribution is defined as follows for $$x \geq 0$$.

$$
P(X < x) = F(x) = 1 - e^{-\lambda x}
$$

If $$x < 0$$, then $$F(x) = 0$$.

This is how the cumulative distribution function is derived.

$$
F(x) = \int_{a}^{b} f(x)\ dx \\
F(x) = \int_{a}^{b} \lambda e^{-\lambda x}\ dx \\
F(x) = [-\frac{1}{\lambda} \lambda e^{-\lambda x}]_{a}^{b} \\
F(x) = [-e^{-\lambda x}]_{a}^{b}
$$

Given that the probability is the area under the curve $$f(x)$$, then the total area under the curve from the interval $$[0, \infty)$$ is as follows.

$$
\int_{0}^{\infty} f(x)\ dx = \int_{0}^{\infty} \lambda e^{-\lambda x}\ dx = 1
$$

And for the interval $$[0, 0]$$ we're going to get this.

$$
\int_{0}^{0} f(x)\ dx = \int_{0}^{0} \lambda e^{-\lambda x}\ dx = 0
$$

Therefore, we get the probability for $$X < x$$ as follows.

$$
P(X < x) = \int_{0}^{x} f(x)\ dx \\
P(X < x) = [-e^{-\lambda x}]_{0}^{x} \\
P(X < x) = -e^{-\lambda x} - (-e^{-\lambda (0)}) \\
P(X < x) = -e^{-\lambda x} - (-e^0) \\
P(X < x) = -e^{-\lambda x} - (-1) \\
P(X < x) = 1 - e^{-\lambda x} \\
$$

Note that I'm taking [this note](http://www.math.wm.edu/~leemis/probability/samplepages/page257.pdf) from the College of William & Mary as a reference in the integration process.

Since $$P(X < x) + P(X > x) = 1$$, we can define $$P(X > x)$$ as follows.

$$
P(X < x) = 1 - e^{-\lambda x} \\
P(X < x) - 1 = -e^{-\lambda x} \\
P(X < x) - (P(X < x) + P(X > x)) = -e^{-\lambda x} \\
P(X < x) - P(X < x) - P(X > x) = -e^{-\lambda x} \\
-P(X > x) = -e^{-\lambda x} \\
P(X > x) = e^{-\lambda x}
$$

# Calculating the Probability

So if we have an event happening at the rate of $$\lambda = 3$$ occurences per hour, the expected wait time between two events happening is $$\mu = \frac{1}{3}$$ hour which is equal to 20 minutes. Let's say we want to calculate the probability of two events happening within a 5-minute interval, so $$x = \frac{5}{60}= \frac{1}{12}$$ hour.

$$
P(X < \frac{1}{12}) = 1 - e^{-\lambda x} \\
P(X < \frac{1}{12}) = 1 - e^{-3(\frac{1}{12})} \\
P(X < \frac{1}{12}) = 1 - e^{-\frac{3}{12}} \\
P(X < \frac{1}{12}) = 1 - e^{-\frac{1}{4}} \\
P(X < \frac{1}{12}) = 1 - 0.7788 \\
P(X < \frac{1}{12}) = 0.2212
$$

If we have an event is expected to happen every 15 minutes, we have $$\mu = \frac{1}{4} = 0.25$$ hour. If we want to calculate the probability of two events happening within a 3-minute interval, we have $$x = \frac{3}{60} = \frac{1}{20} = 0.05$$ hour.

$$
P(X < 0.05) = 1 - e^{-\frac{x}{\mu}} \\
P(X < 0.05) = 1 - e^{-\frac{0.05}{0.25}} \\
P(X < 0.05) = 1 - e^{-\frac{0.05}{0.25}} \\
P(X < 0.05) = 1 - e^{-\frac{1}{5}} \\
P(X < 0.05) = 1 - 0.8187 \\
P(X < 0.05) = 0.1813
$$

And the probability for the said two events to happen with more than 30-minute interval between them is as follows, with $$x = \frac{30}{60} = 0.5$$.

$$
P(X > 0.5) = e^{-\frac{x}{\mu}} \\
P(X > 0.5) = e^{-\frac{0.5}{0.25}} \\
P(X > 0.5) = e^{-2} \\
P(X > 0.5) = 0.1353
$$

# Extra Notes

This [Wikipedia article](https://en.wikipedia.org/wiki/Exponential_distribution) also explains about the moments of the exponential distribution. We're not covering the moments here.

# Weibull Distribution

The probability density function of the Weibull distribution is defined as follows.

$$
f(x) = \frac{k}{\lambda}(\frac{x}{\lambda})^{k - 1} e^{-(\frac{x}{\lambda})^k}
$$

According to [this video](https://www.youtube.com/watch?v=8M_VRCc9rMY), the exponential distribution can be modeled as a special case of the Weibull distribution. But it seems that's not really something special, as the Weibull distribution's flexibility let it model other distributions such as [normal distribution](https://en.wikipedia.org/wiki/Normal_distribution) and [Rayleigh distribution](https://en.wikipedia.org/wiki/Rayleigh_distribution) too.

The Weibull distribution isn't covered in this article, but I plan to read more about it and make another article as a note for Weibull distribution.

# References

[Understanding and Choosing the Right Probability Distributions](https://onlinelibrary.wiley.com/doi/pdf/10.1002/9781119197096.app03)

[Advanced Analytical Models: Over 800 Models and 300 Applications from the Basel II Accord to Wall Street and Beyond](https://www.amazon.com/Advanced-Analytical-Models-Applications-Accord/dp/047017921X)

[Exponential and Weibull Distributions](http://www.thphys.nuim.ie/Notes/EE304/Notes/LEC10/ExpWeibull_handout.pdf)

[The Exponential Distribution](https://openstax.org/books/introductory-business-statistics/pages/5-3-the-exponential-distribution)

[Exponential Distribution](http://www.math.wm.edu/~leemis/probability/samplepages/page257.pdf)

[Exponential distribution](https://en.wikipedia.org/wiki/Exponential_distribution)

[Exponential & Weibull Distribution: Illustration with practical examples](https://www.youtube.com/watch?v=8M_VRCc9rMY)

[Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution)

[Rayleigh distribution](https://en.wikipedia.org/wiki/Rayleigh_distribution)
