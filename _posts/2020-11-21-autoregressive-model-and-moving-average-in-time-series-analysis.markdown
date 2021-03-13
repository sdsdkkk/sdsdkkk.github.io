---
layout: post
title: "Autoregressive Model and Moving Average in Time Series Analysis"
date: 2020-11-21 00:00:00
description: An overview of the autoregressive model and moving average
tags: Statistics
---

[Time series analysis](https://towardsdatascience.com/the-complete-guide-to-time-series-analysis-and-forecasting-70d476bfe775) is the analysis of time series data, which is ordered by time and is collected in a fixed time intervals. The goal of time series analysis is to be able to predict the value of a data point at a point in time in the future.

There are a few properties of time series data that can be used for predictions, such as [seasonality](https://en.wikipedia.org/wiki/Seasonality), [stationarity](https://towardsdatascience.com/stationarity-in-time-series-analysis-90c94f27322), and [autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation). There are a few variations of the list of what properties time series data has, but I'm going to use [this video](https://www.youtube.com/watch?v=GUq_tO2BjaU) as reference. It mentions that the components are trend, seasonality, cycle, and irregularity.

This [lecture slides](http://web.vu.lt/mif/a.buteikis/wp-content/uploads/2019/02/Lecture_03.pdf) by Andius Buteikis from Vilniaus Universitetas mentions that the general mathematical representation for time series decomposition can be defined as follows.

$$
Y_t = f(T_t, S_t, E_t)
$$

Where $$Y_t$$ is the time series value at period $$t$$, $$T_t$$ is a deterministic trend-cycle or general movement component, $$S_t$$ is a deterministic seasonal component, an $$E_t$$ is the irregular stationary component.

Stationarity is having a relatively fixed statistical properties over the generated data over a period of time, such as having a fixed range of mean or standard deviation of the data. One of the example of stationarity within a time series data is [white noise](https://machinelearningmastery.com/white-noise-time-series-python/).

[This article](https://medium.com/quantyca/a-gentle-introduction-to-time-series-7e9f5418dad8) mentions that there are two categories of stationarity: strict stationarity and weak stationarity.

Strict stationarity is found when the entire probability distribution of the time series value doesn't vary with time shift. Weak stationarity is found when the mean $$\mu_t$$ and the variance $$\sigma_{t}^2$$ don't change over time, and the autocovariance $$\gamma(t', t'')$$ of the time series values is only a function of temporal distance between the two random variables of time $$t'$$ and $$t''$$.

# Autoregressive Model

[Autoregressive model](https://en.wikipedia.org/wiki/Autoregressive_model) is a representation of a random process in time series data. The notation $$AR(p)$$ indicates an autoregressive model of order $$p$$.

The $$AR(p)$$ model is defined as follows.

$$
X_t = c + \sum_{i = 1}^p \phi_i X_{t - 1} + \epsilon_t
$$

Where $$\phi_1, ..., \phi_p$$ are the parameters of the model, $$c$$ is a constant, and $$\epsilon_t$$ is white noise. The same equation can be rewritten using the [backshift operator](https://en.wikipedia.org/wiki/Lag_operator) $$B$$ as follows.

$$
X_t = c + \sum_{i = 1}^p \phi_i B^i X_t + \epsilon_t \\
X_t - \sum_{i = 1}^p \phi_i B^i X_t = c + \epsilon_t \\
(1 - \sum_{i = 1}^p \phi_i B^i) X_t = c + \epsilon_t
$$

We can define the function $$\Phi$$ for the backshift operator $$B$$ as follows.

$$
\Phi(B) = 1 - \sum_{i = 1}^p \phi_i B^i
$$

Therefore, the equation can be written as the following.

$$
\Phi(B) X_t = c + \epsilon_t
$$

[This video](https://www.youtube.com/watch?v=5-2C4eO4cPQ) gives an explanation on autoregressive models. We can use autocorrelation function (ACF) and partial autocorrelation function (PACF) to check if the data points are autocorrelated and help determine the lag period and $$\phi$$ coefficients we can use in the autoregressive model, as explained in [this video](https://www.youtube.com/watch?v=DeORzP0go5I).

# Moving Average

[Moving average](https://en.wikipedia.org/wiki/Moving_average) is a method of calculating average values in time series data for the last $$n$$ data points before the time $$t$$. The simplest implementation is simply taking the last $$n$$ data points from $$t$$ and calculate the mean.

$$
\bar{p}_{t} = \frac{p_t + p_{t - 1} + ... + p_{t - (n - 1)}}{n} = \frac{1}{n} \sum_{i = 0}^{n - 1}p_{t - i}
$$

To calculate the successive period's average $$\bar{p}$$ we can simply use this formula, given we've already calculated the previous period's value.

$$
\bar{p}_{t} = \bar{p}_{t - 1} + \frac{1}{n} (p_t - p_{t - n})
$$

There are adjustments that can be made with the moving average formula such as adding different weights to each values before it's being averaged. The [Wikipedia page](https://en.wikipedia.org/wiki/Moving_average) contains some examples of them.

Moving averages can be used to calculate the mean of the values within a period of time, and the mean can be used to estimate a data point value close to that period. For that, we can use the [moving average model](https://en.wikipedia.org/wiki/Moving-average_model).

The notation for moving average model $$MA(q)$$ where $$q$$ is the order of of the model can be defined as follows.

$$
X_t = \mu + \epsilon_t + \theta_1 \epsilon_{t - 1} + ... + \theta_q \epsilon_{t - q} \\
X_t = \mu + \epsilon_t + \sum_{i = 1}^q \theta_i \epsilon_{t - i}
$$

Where $$\mu$$ is the mean of the series, $$\theta_1, ..., \theta_q$$ are the parameters of the model, and $$\epsilon_t, \epsilon_{t - 1}, ..., \epsilon_{t - q}$$ are the white noise error terms. We can write the same equation using the backshift operator $$B$$ as follows.

$$
X_t = \mu + (1 + \theta_1 B + ... + \theta_q B^q) \epsilon_t \\
X_t = \mu + (1 + \sum_{i = 1}^q \theta_i B^i) \epsilon_t
$$

# Combining Autoregressive Model and Moving Average

Autoregressive models can be used to predict a data point value in a time series data given that we have enough data to build a model for the time series patterns based on the data. Moving average can be used to calculate the average of the data in a time period to be integrated to the autoregressive model in order to improve the prediction.

There are a few methods to combine the use of autoregressive model and moving average such as [ARMA](https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model), [ARIMA](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average), and [SARIMA](https://towardsdatascience.com/understanding-sarima-955fe217bc77). We're going to look at the ARMA model for an example.

An ARMA model $$ARMA(p, q)$$ is basically a combination of the autoregressive model $$AR(p)$$ and the moving average model $$MA(q)$$, as we can see the function below.

$$
X_t = c + \epsilon_t + \sum_{i = 1}^p \phi_i X_{t - i} + \sum_{i = 1}^q \theta_i \epsilon_{t - i}
$$

[This reference](https://www.statsmodels.org/stable/examples/notebooks/generated/tsa_arma_0.html) provides an example of ARMA model implementation using Python with [statsmodels](https://www.statsmodels.org/).

The other variations of integrating autoregressive model and moving average model add and modify some things in the mathematical formula to fit the data model better. Understanding the characteristics of the data and the models are required to pick the model best-suited for the data.

# References

[The Complete Guide to Time Series Analysis and Forecasting](https://towardsdatascience.com/the-complete-guide-to-time-series-analysis-and-forecasting-70d476bfe775)

[Seasonality](https://en.wikipedia.org/wiki/Seasonality)

[Stationarity in time series analysis](https://towardsdatascience.com/stationarity-in-time-series-analysis-90c94f27322)

[Autocorrelation](https://en.wikipedia.org/wiki/Autocorrelation)

[Introducing Time Series Analysis and forecasting](https://www.youtube.com/watch?v=GUq_tO2BjaU)

[03 Time series with trend and seasonality components](http://web.vu.lt/mif/a.buteikis/wp-content/uploads/2019/02/Lecture_03.pdf)

[White Noise Time Series with Python](https://machinelearningmastery.com/white-noise-time-series-python/)

[A gentle introduction to Time Series](https://medium.com/quantyca/a-gentle-introduction-to-time-series-7e9f5418dad8)

[Autoregressive model](https://en.wikipedia.org/wiki/Autoregressive_model)

[Lag operator](https://en.wikipedia.org/wiki/Lag_operator)

[Time Series Talk : Autoregressive Model](https://www.youtube.com/watch?v=5-2C4eO4cPQ)

[Time Series Talk : Autocorrelation and Partial Autocorrelation](https://www.youtube.com/watch?v=DeORzP0go5I)

[Moving average](https://en.wikipedia.org/wiki/Moving_average)

[Moving-average model](https://en.wikipedia.org/wiki/Moving-average_model)

[Autoregressive–moving-average model](https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model)

[Autoregressive integrated moving average](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average)

[Understanding SARIMA (More Time Series Modeling)](https://towardsdatascience.com/understanding-sarima-955fe217bc77)

[Autoregressive Moving Average (ARMA): Sunspots data](https://www.statsmodels.org/stable/examples/notebooks/generated/tsa_arma_0.html)

[stasmodels](https://www.statsmodels.org/)
