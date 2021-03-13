---
layout: post
title: "Autocorrelation: ACF and PACF"
date: 2020-12-10 00:00:00
description: How to calculate values on ACF and PACF plots for lags in time series data
tags: Statistics
---

[Autocorrelation function](https://en.wikipedia.org/wiki/Autocorrelation) (ACF) is defined as the correlation of a signal $$S_t$$ with a delayed copy as itself $$S_{t + k}$$ as a function of delay where we're measuring the indirect correlation between them (the intermediary correlation between lag $$t$$ and lag $$t + k$$ included in the calculation). When we're measuring only the direct correlation between the signal $$S_t$$ to the signal $$S_{t + k}$$ while disregarding the correlation factor with the intermediary lags $$n$$ where $$0 < n < k$$, we're using the [partial autocorrelation function](https://en.wikipedia.org/wiki/Partial_autocorrelation_function) (PACF). Autocorrelation is [commonly used](https://towardsdatascience.com/significance-of-acf-and-pacf-plots-in-time-series-analysis-2fa11a5d10a8) on time series analysis.

[This video](https://www.youtube.com/watch?v=DeORzP0go5I) gives a short explanation about ACF and PACF. [This article](http://methods.sagepub.com/base/download/DatasetStudentGuide/time-series-acf-pacf-in-us-feedgrains-1876-2015) explains the difference between ACF and PACF, also shows an example of how to calculate the values for ACF and PACF for a few sample lags.

# Autocorrelation Function

Since autocorrelation is basically the correlation of a time series data with its delayed value, we can use the formula of [correlation](https://en.wikipedia.org/wiki/Correlation_and_dependence) as follows.

$$
corr(X, Y) = \frac{cov(X, Y)}{\sigma_X \sigma_Y} \\
corr(X, Y) = \frac{E[(X - \mu_X) (Y - \mu_Y)]}{\sigma_X \sigma_Y} \\
corr(X, Y) = \frac{E[X, Y] - E[X] E[Y]}{\sqrt{E[X^2] - E[X]^2} \sqrt{E[Y^2] - E[Y]^2}}
$$

For signal $$S$$ in time $$t$$ and lag $$n$$, we can assign $$X = S_{t + 1}$$ and $$Y = S_t$$ in the equation.

The value for ACF at lag 0 is always 1, since the correlation between $$S_t$$ and itself is 1. The ACF plot for each lags can be shown as follows.

| Lag     | Correlation Value              |
|---------|--------------------------------|
| 0       | 1                              |
| 1       | $$corr(S_{t + 1}, S_t)$$       |
| 2       | $$corr(S_{t + 2}, S_{t + 1})$$ |
| 3       | $$corr(S_{t + 3}, S_{t + 2})$$ |
| 4       | $$corr(S_{t + 4}, S_{t + 3})$$ |

ACF plots are commonly used as a reference to determine the parameters and lags for [moving average models](https://en.wikipedia.org/wiki/Moving-average_model).

# Partial Autocorrelation Function

Partial autocorrelation also utilizes the same correlation formula to calculate the autocorrelation between the lags. But for PACF we want to disregard the indirect correlation between $$S_t$$ and $$S_{t + k}$$ for the lag $$n$$ through the range of $$ 0 < n < k$$. For that, we use the following formula to calculate the autocorrelation.

Given that $$k$$ is the lag, we can calculate the PACF for $$k = 1$$ as follows.

$$
\alpha(1) = corr(S_{t + 1}, S_t)
$$

Given that $$k \ge 2$$, we can use the following formula.

$$
\alpha(k) = corr(S_{t - k} - P_{t, k}(S_{t + k}), S_t - P_{t, k}(S_t))
$$

Given that $$P_{t, k}(x)$$ is the surjective operator of the orthogonal projection of $$x$$ onto the linear subspace of [Hilbert space](https://en.wikipedia.org/wiki/Hilbert_space) spanned by $$S_{t + 1}, ... , S_{t + k}$$, as explained in this [Wikipedia article](https://en.wikipedia.org/wiki/Partial_autocorrelation_function).

The value for PACF for each lags are as follows.

| Lag     | Correlation Value        |
|---------|--------------------------|
| 0       | 1                        |
| 1       | $$\alpha(1)$$            |
| 2       | $$\alpha(2)$$            |
| 3       | $$\alpha(3)$$            |
| 4       | $$\alpha(4)$$            |

PACF plots are commonly used as a reference to determine parameters and lags for [autoregressive models](https://en.wikipedia.org/wiki/Autoregressive_model).

# Extra Notes

Machine Learning Mastery published [this article](https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/) explaining ACF and PACF with example code snippets in Python.

[This article](https://www.mathworks.com/help/econ/autocorrelation-and-partial-autocorrelation.html) explains about the ACF and PACF functions provided by Matlab. [This one](https://rinterested.github.io/statistics/acf_pacf.html) shows the example using R and with some explanations on how ACF and PACF works.

[This LinkedIn article](https://www.linkedin.com/pulse/reading-acf-pacf-plots-missing-manual-cheatsheet-saqib-ali/) shows how to read ACF and PACF plots.

# References

[Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation)

[Partial autocorrelation function](https://en.wikipedia.org/wiki/Partial_autocorrelation_function)

[Significance of ACF and PACF Plots In Time Series Analysis](https://towardsdatascience.com/significance-of-acf-and-pacf-plots-in-time-series-analysis-2fa11a5d10a8)

[Time Series Talk : Autocorrelation and Partial Autocorrelation](https://www.youtube.com/watch?v=DeORzP0go5I)

[Learn About Time Series ACF and PACF in SPSS With Data From the USDA Feed Grains Database (1876–2015)](http://methods.sagepub.com/base/download/DatasetStudentGuide/time-series-acf-pacf-in-us-feedgrains-1876-2015)

[Correlation and dependence](https://en.wikipedia.org/wiki/Correlation_and_dependence)

[Moving-average model](https://en.wikipedia.org/wiki/Moving-average_model)

[Hilbert space](https://en.wikipedia.org/wiki/Hilbert_space)

[Autoregressive model](https://en.wikipedia.org/wiki/Autoregressive_model)

[A Gentle Introduction to Autocorrelation and Partial Autocorrelation](https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/)

[Autocorrelation and Partial Autocorrelation](https://www.mathworks.com/help/econ/autocorrelation-and-partial-autocorrelation.html)

[How to calculate manually ACF and PACFs](https://rinterested.github.io/statistics/acf_pacf.html)

[Reading the ACF and PACF Plots - The Missing Manual / Cheatsheet](https://www.linkedin.com/pulse/reading-acf-pacf-plots-missing-manual-cheatsheet-saqib-ali/)
