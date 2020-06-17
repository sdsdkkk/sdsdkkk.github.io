---
layout: content
title: "Basic Concepts on Statistical Hypothesis Testing"
date: 2020-06-17 00:00:00
description: Some factors that should be considered when inferring from statistical data
---

[Hypothesis testing](https://machinelearningmastery.com/statistical-hypothesis-tests/) is a method to statistically test our hypothesis regarding a population based on the observation data we collected. A [null hypothesis](https://en.wikipedia.org/wiki/Null_hypothesis) is the general statement that there is no relationship between two measured phenomena or no association between two groups.

In order to perform hypothesis testing, there are a few things that need to be understood. We're generally assuming populations with normal distributions when performing hypothesis testing but seems like hypothesis testing's application isn't limited to normal distributions as there's [this paper](https://link.springer.com/article/10.1631/jzus.C12a0241) on applying hypothesis testing on [Weibull distribution](https://en.wikipedia.org/wiki/Weibull_distribution).

# Null Hypothesis and Alternative Hypothesis

A null hypothesis is usually denoted as $$H_0$$, while an alternative hypothesis is usually denoted as $$H_1$$. We're taking an example case from [this Investopedia article](https://www.investopedia.com/terms/n/null_hypothesis.asp) regarding how to determine a null hypothesis and an alternative hypothesis.

Here's the case we're taking from the linked Investopedia article. A school principal claims that the students at her school scored 7 out of 10 on average in exams. So we're expecting the mean of the population $$\mu = 7.0$$ regarding their test scores.

Suppose we're trying to test the principal's claim by taking a sample size of $$n$$ students to measure their test scores. The null hypothesis $$H_0$$ of this test is that the mean score of the overall population is $$\mu = 7.0$$, and the alternative hypothesis is that the mean score of the overall population is $$\mu \neq 7.0$$.

So we can try to perform a statistical test on sample populations fro the students to try disproving the claim by the principal that the the average score of the students in the school is really 7 out of 10.

Null hypothesis is usually defined with the assumed status quo being correct or there's no statistical significance in the tests being performed.

# p-value

[p-value](https://en.wikipedia.org/wiki/P-value) is defined as the probability of obtaining test results at least as extreme as the result observed during the test given that the null hypothesis is true. Usually, we're using $$\alpha = 0.05$$ as the baseline significance level in order to make the decision to reject or not to reject the null hypothesis.

It's important to not arbitrarily make adjustments to the research plan based on the obtained p-value from test results in order to force the test data to give us a more desirable p-value. It's called [p-hacking](https://bitesizebio.com/31497/guilty-p-hacking/) as is considered a scientific misconduct.

Back to the principal's claim that her students average exam score is $$\mu = 7.0$$. Now that we have a sample population $$X$$ with the size of $$n$$, we can estimate the distribution of student exam scores of the entire population based on our observation on the sample students' exam score. We assign the p-value to the observation result based on the probability of the observation to deviate from the null hypothesis merely by chance of random sampling.

Using $$\alpha = 0.05$$ as the baseline, we consider the probability of the null hypothesis $$H_0$$ being true or false based on the probability of us having the exact sample distribution as we have under the assumption that $$H_0$$ is true. p-value $$p \leq \alpha$$ means that given the null hypothesis is true, there's less than 5% of probability for us having the exact distribution of our sample by random chance (which means that our sample is on the either end of the extreme tails of the entire population's estimated distribution).

If we reject the null hypothesis $$H_0$$, that means we accept the alternative hypothesis $$H_1$$ to be correct. Yet, p-value $$p \leq \alpha$$ doesn't really shows whether the null hypothesis is correct or not but only gives us the probability of us having the exact result of our statistical test given that the null hypothesis is true. So we might be rejecting the null hypothesis by the mistake of having taken a sample set nearing one extreme end of the claimed entire population distribution's tail.

Also, having $$p > \alpha$$ doesn't really prove that the null hypothesis is true. It's just that assuming the null hypothesis is true, the test result we get has 95% chance to fall within the non-extreme ranges of the assumed population distribution's curves with $$\alpha = 0.05$$. So statistically, there's a good chance that the null hypothesis is still true despite the deviation of the sample set from the null hypothesis.

This brings us to type I and type II errors:

- **Type I error:** incorrect rejection of the null hypothesis
- **Type II error:** incorrect acceptance of the null hypothesis

# Power, Effect, and Sample Size

From [this book](https://www.amazon.com/Essential-Guide-Effect-Sizes-Interpretation/dp/0521142466/) as quoted in [this article](https://machinelearningmastery.com/statistical-power-and-power-analysis-in-python/) on Machine Learning Mastery, statistical power is the probability that a test will correctly reject a false null hypothesis and is only relevant is the null hypothesis is false. Power is defined as follows given that $$\beta$$ is the probability of type II error.

$$
power = P(reject\ H_0 | H_1\ true) = 1 - \beta
$$

Note that if $$H_1$$ is true, $$H_0$$ must be false. The higher the power the smaller our risk of committing type II error.

According to [this Wikipedia article](https://en.wikipedia.org/wiki/Power_of_a_test), statistical power depends on the following three factors:

- The statistical significance ($$\alpha$$) used in the test
- The [effect size](https://en.wikipedia.org/wiki/Effect_size) of the population
- The sample size ($$n$$) used to detect the effect

The statistical significance $$\alpha$$ is determined from the beginning, in our case we're using $$\alpha = 0.05$$.

Effect size can be calculated using [Cohen's $$d$$](https://en.wikiversity.org/wiki/Cohen%27s_d), since we're trying to calculate the effect size between two populations' means relative to their standard deviations. The larger the value of $$d$$, the more significant the difference between the two populations. The formula is as follows.

$$
d = \frac{\bar{x_1} - \bar{x_2}}{s}
$$

Where $$s$$ is the standard deviation of the population from which the groups are sampled.

Since we're assuming two populations with different observed expected values $$\bar{x_1}$$ and $$\bar{x_2}$$, we should have the standard deviations $$s_1$$ and $$s_2$$ for each respective populations that can be pooled. [Jacob Cohen](https://en.wikipedia.org/wiki/Jacob_Cohen_(statistician)) defined $$s$$ as the pooled standard deviation as follows.

$$
s = \sqrt{\frac{(n_1 - 1) s_{1}^2 + (n_2 - 1) s_{2}^2}{n_1 + n_2 - 2}}
$$

But according to [this Wikipedia article on effect size](https://en.wikipedia.org/wiki/Effect_size), some people have a slightly different way to calculate $$s$$ for Cohen's $$d$$.

$$
s = \sqrt{\frac{(n_1 - 1) s_{1}^2 + (n_2 - 1) s_{2}^2}{n_1 + n_2}}
$$

[This video](https://www.youtube.com/watch?v=tTgouKMz-eI) straight up uses the average standard deviation between the two populations in order to find $$s$$.

$$
s = \frac{s_1 + s_2}{2}
$$

[This article](https://www.statisticshowto.com/pooled-standard-deviation/) from Statistics How To claims that a simple method of pooling should be enough so there's not really much advantage to use Cohen's pooled standard deviation formula over simpler ones. Their recommended formula is this one.

$$
s = \sqrt{\frac{s_{1}^2 + s_{2}^2}{2}}
$$

[This video](https://www.youtube.com/watch?v=IetVSlrndpI) though, while agrees to use the formula recommended in the Statistics How To article, recommends to use the original formula by Jacob Cohen when comparing two populations with unequal sizes.

It seems that the usual recommended value for $$d$$ is 0.8. Based on the value of power, $$\alpha$$, and $$d$$ that we expect from the test, we can use [the power table](https://www.pilesofvariance.com/Chapter13/Cohen_Power_Tables.pdf) to determine how many samples per test we should take. The power table seems to assume an extremely large number of individuals in the source population though.

# References

[A Gentle Introduction to Statistical Hypothesis Testing](https://machinelearningmastery.com/statistical-hypothesis-tests/)

[Null hypothesis](https://en.wikipedia.org/wiki/Null_hypothesis)

[Hypothesis testing for reliability with a three-parameter Weibull distribution using minimum weighted relative entropy norm and bootstrap](https://link.springer.com/article/10.1631/jzus.C12a0241)

[Weibull distribution](https://en.wikipedia.org/wiki/Weibull_distribution)

[Null Hypothesis](https://www.investopedia.com/terms/n/null_hypothesis.asp)

[p-value](https://en.wikipedia.org/wiki/P-value)

[Are you Guilty of P-Hacking?](https://bitesizebio.com/31497/guilty-p-hacking/)

[The Essential Guide to Effect Sizes: Statistical Power, Meta-Analysis, and the Interpretation of Research Results 1st Edition](https://www.amazon.com/Essential-Guide-Effect-Sizes-Interpretation/dp/0521142466/)

[A Gentle Introduction to Statistical Power and Power Analysis in Python](https://machinelearningmastery.com/statistical-power-and-power-analysis-in-python/)

[Power of a test](https://en.wikipedia.org/wiki/Power_of_a_test)

[Effect size](https://en.wikipedia.org/wiki/Effect_size)

[Cohen's d](https://en.wikiversity.org/wiki/Cohen%27s_d)

[Jacob Cohen (statistician)](https://en.wikipedia.org/wiki/Jacob_Cohen_(statistician))

[How to calculate Cohen d effect size](https://www.youtube.com/watch?v=tTgouKMz-eI)

[Pooled Standard Deviation](https://www.statisticshowto.com/pooled-standard-deviation/)

[What Is And How To Calculate Cohen's d?](https://www.youtube.com/watch?v=IetVSlrndpI)

[Power Tables for Effect Size d](https://www.pilesofvariance.com/Chapter13/Cohen_Power_Tables.pdf)
