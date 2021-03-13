---
layout: post
title: "Calculating and Validating Granger Causality"
date: 2020-12-02 00:00:00
description: Causal inference using Granger causality
tags: Statistics
---

[Granger causality](http://www.scholarpedia.org/article/Granger_causality) is a statistical concept of causality where if a time series variable $$X$$ Granger causes a time series variable $$Y$$, then past values of $$X$$ should contain information that helps to predict $$Y$$.

We use the term "Granger cause" when describing the causal relation between the two variables because we don't really conclude that the events in $$X$$ actually cause the events in $$Y$$, but the events in $$X$$ that happen prior to the events in $$Y$$ are correlated.

# Granger Causality Model

The model for Granger causality between $$X$$ and $$Y$$ can be written as follows, where $$X$$ Granger causes $$Y$$.

$$
Y_t = c + \sum_{i = 1}^p \phi_i Y_{t - i} + \sum_{i = 1}^q \theta_i X_{t - i} + \epsilon_t
$$

Where $$c$$ is a constant, $$X_t$$ is the value of time series variable $$X$$ at time $$t$$, $$Y_t$$ is the value of time series variable $$Y$$ at time $$t$$, $$\phi$$ is the weight for a past value of $$Y$$, $$\theta$$ is the weight for a past value of $$X$$, and $$\epsilon_t$$ is the prediction error of the time series model.

The formula for the model itself is basically an [autoregressive model](https://en.wikipedia.org/wiki/Autoregressive_model) of $$Y$$ combined with $$X$$, as we can see the similarity with the autoregressive model formula for $$AR(p)$$ of $$Y$$.

$$
Y_t = c + \sum_{i = 1}^p \theta_i Y_{t - i} + \epsilon_t
$$

We can say that $$X$$ Granger causes $$Y$$ if the Granger causality model for $$X$$ Granger causes $$Y$$ proves to provide more accurate predictions than the autoregressive model of $$Y$$. For this, we need to perform a [validation](https://en.wikipedia.org/wiki/Regression_validation) on the Granger causality model.

# Model Validation

[This video](https://www.youtube.com/watch?v=6dOnNNxRJuY) from Andrei Galanchuk shows an example of how to test for Granger causality between two time series variables. The video calculates the F-ratio by using the [coefficient of determination](https://en.wikipedia.org/wiki/Coefficient_of_determination) $$R^2$$.

The method used in the video to perform the validation is the [F-test](https://en.wikipedia.org/wiki/F-test), which is often used when comparing statistical models to test whether the variance between the models are significant enough for us to conclude whether any of the models are significantly better statistically.

The following is the formula for F-ratio.

$$
F = \frac{V_{between}}{V_{within}}
$$

Where $$V_{between}$$ is the variability between groups, and $$V_{within}$$ is the variability within groups. Both variabilities are defined as follows.

$$
V_{between} = \frac{\sum_{i = 1}^K n_i (\bar{Y}_i - \bar{Y})^2}{K - 1} \\
V_{within} = \frac{\sum_{i = 1}^K \sum_{j = 1}^{n_i} (Y_{ij} - \bar{Y}_i)^2}{N - K}
$$

[This video](https://www.youtube.com/watch?v=Xg8_iSkJpAE) from the Khan Academy gives a more detailed explanation on F-test. If $$V_{between} > V_{within}$$ there's a lower probability that the null hypothesis, that the variance between groups is just by random chance, is correct. If $$V_{between} < V_{within}$$, since the variance within a group is higher than the variance between groups there is a higher probability that the variance between groups are just random which translates to the higher probability that the null hypothesis is correct.

We then calculate the p-value from the [F-distribution](https://en.wikipedia.org/wiki/F-distribution) with the F-ratio and the degrees of freedom on the numerator and denominator as the parameters. We can use calculators such as [this one](https://www.socscistatistics.com/pvalues/fdistribution.aspx) to make it easier.

# Extra Notes

[This video](https://www.youtube.com/watch?v=g53UoGllJSs) by Miklesh Yadav shows an example of Granger causality test using R.

[This material](http://amath2.nchu.edu.tw/honda/605Lecture/Lecture%2005_addendum_Chisquare,t%20and%20F%20distributions.PDF) from the National Chung Hsing University shows the relations between chi-square distribution, t-distribution, and F-distribution. [This material](https://mathcs.clarku.edu/~djoyce/ma218/tandfdistributions.pdf) from the Clark University explains about t-distribution and F-distribution.

# References

[Granger causality](http://www.scholarpedia.org/article/Granger_causality)

[Autoregressive model](https://en.wikipedia.org/wiki/Autoregressive_model)

[Regression validation](https://en.wikipedia.org/wiki/Regression_validation)

[STATISTICS I Time Series I Granger Causality Test I Intuition and Example](https://www.youtube.com/watch?v=6dOnNNxRJuY)

[Coefficient of determination](https://en.wikipedia.org/wiki/Coefficient_of_determination)

[F-test](https://en.wikipedia.org/wiki/F-test)

[ANOVA 3: Hypothesis test with F-statistic \| Probability and Statistics \| Khan Academy](https://www.youtube.com/watch?v=Xg8_iSkJpAE)

[F-distribution](https://en.wikipedia.org/wiki/F-distribution)

[P-Value from F-Ratio Calculator (ANOVA)](https://www.socscistatistics.com/pvalues/fdistribution.aspx)

[11.12: Granger Causality Test in RStudio](https://www.youtube.com/watch?v=g53UoGllJSs)

[Chi-square, t-, and F-Distributions (and Their Interrelationship)](http://amath2.nchu.edu.tw/honda/605Lecture/Lecture%2005_addendum_Chisquare,t%20and%20F%20distributions.PDF)

[The t and F distributions](https://mathcs.clarku.edu/~djoyce/ma218/tandfdistributions.pdf)
