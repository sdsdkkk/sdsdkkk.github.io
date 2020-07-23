---
layout: content
title: "Hazard Function in Survival Analysis"
date: 2020-07-23 00:00:00
description: The basic ideas of hazard and cumulative hazard functions in survival analysis
---

I'm using [this video](https://www.youtube.com/watch?v=0a-BF036Di4) as my introductory lesson to the hazard function. The hazard function can be defined as follows.

$$
h(t) = \frac{f(t)}{S(t)}
$$

Given that $$S(t)$$ is the survival function (or the reliability function) of the statistical distribution and $$f(t)$$ is the probability density function, here's how to find $$S(t)$$.

$$
F(t) = \int f(t)\ dt \\
S(t) = 1 - F(t)
$$

The hazard function is used to calculate the hazard rate, which is defined as the rate of failure at a certain point in time.

# Hazard Rate

The hazard rate can also be called as the failure rate if the context is engineering, the force of mortality if the context is actuaries, or the age-specific death rate in life sciences. [This monograph](http://geb.uni-giessen.de/geb/volltexte/2014/10793/pdf/RinneHorst_hazardrate_2014.pdf) by Professor Horst Rinne from the University of Giessen contains the mathematical construct of the hazard rate function in the first chapter starting on page 9.

For the explanation in this section, I'll be mainly taking [this lecture notes](http://people.stat.sfu.ca/~raltman/stat402/402L32.pdf) by Associate Professor Rachel MacKay Altman from the Simon Fraser University due to the simplicity.

Given that the probability density function of the distribution is $$f(t)$$ and $$X$$ is the survival time, the cumulative distribution function can be defined as follows for the probability of failure by the time $$x$$.

$$
F(x) = P(X \leq x) = \int_{0}^{x} f(t)\ dt
$$

As $$F(x)$$ is the probability of failure by the time $$x$$, we can define the probability of survival by the time $$x$$ as follows.

$$
S(x) = 1 - F(x)
$$

Hence, $$S(x)$$ is the survival function or the reliability function of the distribution.

Since the hazard rate is defined as the rate of failure $$f(t)$$ at a certain point in time $$x$$ for the total surviving population $$S(t)$$ at time $$x$$, we can calculate the hazard rate for the point in time $$x$$ as follows.

$$
h(x) = \frac{f(x)}{S(x)}
$$

As the time $$X$$ goes on, the number of the surviving population $$S(x)$$ will become smaller as more individuals from the population have died off.

# Cumulative Hazard Function

Cumulative hazard function is defined as follows.

$$
H(t) = \int h(t)\ dt
$$

Given the time period $$X$$, our cumulative hazard function for the time $$x$$ within the period $$X$$ would be as follows.

$$
H(x) = \int_{0}^{x} h(t)\ dt
$$

The cumulative hazard function isn't a probability function with the range of output from 0 to 1, as we can see [here](http://www.mathwave.com/help/easyfit/html/analyses/graphs/cumulative_hazard.html) the value can surpass 1. But it can be used as a measure of risk. The greater the value of $$H(x)$$ for the point in time $$x$$ translates to the greater risk of failure by the time $$x$$. The problem is that it's harder to interpret as mentioned in [this page](https://www.itl.nist.gov/div898/handbook/apr/section2/apr222.htm) of the engineering statistics handbook by NIST.

The book [An Introduction to Survival Analysis Using Stata, Second Edition](https://www.stata.com/bookstore/survival-analysis-stata-introduction/), which is available on [Google Books](https://books.google.de/books?id=xttbn0a-QR8C&printsec=frontcover&hl=de#v=onepage&q&f=false), explains how to interpret cumulative hazard on page 13-15. From the book's explanation, it seems that the cumulative hazard is a measure of unit of risk of failure within a period of time.

# Extra Resources

There are some videos by Eric Cai on YouTube to help understand hazard functions better mathematically:

- [The Definition of the Hazard Function in Survival Analysis](https://www.youtube.com/watch?v=KM23TDz75Fs)
- [h(t) = f(t) ÷ S(t) - The hazard function is the PDF divided by the survival function](https://www.youtube.com/watch?v=o2DWODTs3xo)

# References

[Hazard Rate and related concepts in Reliability Engineering](https://www.youtube.com/watch?v=0a-BF036Di4)

[The Hazard Rate - Theory and Inference](http://geb.uni-giessen.de/geb/volltexte/2014/10793/pdf/RinneHorst_hazardrate_2014.pdf)

[Lecture 32: Survivor and Hazard Functions](http://people.stat.sfu.ca/~raltman/stat402/402L32.pdf)

[Cumulative Hazard Function](http://www.mathwave.com/help/easyfit/html/analyses/graphs/cumulative_hazard.html)

[Hazard and cumulative hazard plotting](https://www.itl.nist.gov/div898/handbook/apr/section2/apr222.htm)

[An Introduction to Survival Analysis Using Stata, Second Edition](https://www.stata.com/bookstore/survival-analysis-stata-introduction/)

[An Introduction to Survival Analysis Using Stata, Second Edition - Google Books](https://books.google.de/books?id=xttbn0a-QR8C&printsec=frontcover&hl=de#v=onepage&q&f=false)

[The Definition of the Hazard Function in Survival Analysis](https://www.youtube.com/watch?v=KM23TDz75Fs)

[h(t) = f(t) ÷ S(t) - The hazard function is the PDF divided by the survival function](https://www.youtube.com/watch?v=o2DWODTs3xo)
