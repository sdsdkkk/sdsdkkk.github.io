---
layout: content
title: "Deriving Poisson Probability Function from Binomial Distribution"
date: 2020-05-06 00:00:00
description: Looking at how the Poisson probability function is constructed from binomial formula
---

The following is the formula to calculate the probability of an event $$X$$ happening $$k$$ times in a time period given that the rate of occurence of the event in a time period is $$\lambda$$.

$$
P(k) = \frac{\lambda^k e^{- \lambda}}{k!}
$$

$$e$$ is [Euler's number](https://www.mathsisfun.com/numbers/e-eulers-number.html) and is a constant in the equation.

# Poisson Process

Taking the definition from [this article](https://towardsdatascience.com/the-poisson-distribution-and-poisson-process-explained-4e2cb17d459) by Will Koehrsen, a Poisson process is a model for a series of discrete independent events where the average time between the events happening is known but the exact timing of the events happening is random. In another word from [another article](https://www.probabilitycourse.com/chapter11/11_1_2_basic_concepts_of_the_poisson_process.php), the number of occurences of a certain event is quite stable within a regular time period but it is random in terms of the exact moment of the occurence.

The Poisson process is usually used to model events such as the occurences of car accidents, the number of incoming calls in a call center, and the number of server failures for a service within a period.

# Poisson Distribution

The equation at the start of this article is the probability mass function for the Poisson distribution.

$$
f(k, \lambda) = P(X = k) = \frac{\lambda^k e^{- \lambda}}{k!}
$$

Where the components are as follows.

| Component     | Description                        |
|---------------|------------------------------------|
| $$k$$         | Number of occurences in the period |
| $$\lambda$$   | The rate of occurences per period  |
| $$e$$         | Euler's number, a constant         |

As the rate of occurence of the event within the time period, $$\lambda$$ should be the expected value of number of occurences $$k$$ within the length of the time period used for the calculation. We can use the average number of occurences as the value of $$\lambda$$.

I was curious about how the probability function is defined as that function, and found out from [this video](https://www.youtube.com/watch?v=ceOwlHnVCqo) that the formula seems to have some relation to the [binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution) as it can be derived from the binomial distribution's probability mass function with $$n \to \infty$$.

As a note for the later section, $$e^x$$ is equal to the following.

$$
e^x = \lim_{n \to \infty} (1 + \frac{x}{n})^n
$$

# Binomial Distribution

The following is the probability mass function for the binomial distribution.

$$
f(k, n, p) = P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) p^k (1 - p)^{n - k}
$$

Where the components are as follows.

| Component     | Description                        |
|---------------|------------------------------------|
| $$k$$         | Number of hits from all events     |
| $$n$$         | Number of all events               |
| $$p$$         | Probability of a hit per event     |

The [binomial coefficient](https://en.wikipedia.org/wiki/Binomial_coefficient) is defined as follows.

$$\left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) = \frac{n!}{k! (n - k)!}$$

# Binomial to Poisson

Let's start working on the equation to see how the Poisson formula can be derived from binomial probability mass function.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) p^k (1 - p)^{n - k}
$$

$$\lambda$$ as the expected number of hits is $$\lambda = np$$ as $$n$$ is the number of trials and $$p$$ is the probability of hit per trial. So $$p = \frac{\lambda}{n}$$, and we can substitute it in the equation.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) (\frac{\lambda}{n})^k (1 - \frac{\lambda}{n})^{n - k}
$$

Next, we can substitute the binomial coefficient of $$n$$ and $$k$$.

$$
P(X = k) = \frac{n!}{k! (n - k)!} (\frac{\lambda}{n})^k (1 - \frac{\lambda}{n})^{n - k}
$$

We can then rearrange the equation.

$$
P(X = k) = \frac{\lambda^k}{k!} \frac{n!}{(n - k)!} \frac{1}{n^k} (1 - \frac{\lambda}{n})^{n - k}
$$

The operation $$\frac{n!}{(n - k)!} \frac{1}{n^k}$$ can be transformed.

$$
\frac{n!}{(n - k)!} \frac{1}{n^k} = \frac{n(n - 1)(n - 2) ... (n - k + 1)(n - k)!}{(n - k)! n^k} \\
\frac{n!}{(n - k)!} \frac{1}{n^k} = \frac{n(n - 1)(n - 2) ... (n - k + 1)}{n^k} \\
\frac{n!}{(n - k)!} \frac{1}{n^k} = \frac{n}{n} \frac{n - 1}{n} \frac{n - 2}{n} ... \frac{n - k + 1}{n} \\
\frac{n!}{(n - k)!} \frac{1}{n^k} = 1 (1 - \frac{1}{n}) (1 - \frac{2}{n}) ... (1 - \frac{k + 1}{n})
$$

Now we can plug it in for substitution in the $$P(X = k)$$ function.

$$
P(X = k) = \frac{\lambda^k}{k!} (1 - \frac{1}{n}) (1 - \frac{2}{n}) ... (1 - \frac{k + 1}{n}) (1 - \frac{\lambda}{n})^{n - k}
$$

Since $$(1 - \frac{\lambda}{n})^{n - k} =  (1 - \frac{\lambda}{n})^{n} (1 - \frac{\lambda}{n})^{-k}$$, we can substitute it too.

$$
P(X = k) = \frac{\lambda^k}{k!} (1 - \frac{1}{n}) (1 - \frac{2}{n}) ... (1 - \frac{k + 1}{n}) (1 - \frac{\lambda}{n})^{n} (1 - \frac{\lambda}{n})^{-k}
$$

Now we can apply the limit with $$n \to \infty$$. Most of the fractions with $$n$$ as the denominator will be approaching zero since it's divided by $$n$$ which is approaching infinity. But there's a special case with $$(1 - \frac{\lambda}{n})^{n}$$, as described in the previous section about $$e^x$$ it will become as follows.

$$
\lim_{n \to \infty} (1 - \frac{\lambda}{n})^{n} = \lim_{n \to \infty} (1 + \frac{- \lambda}{n})^{n} = e^{- \lambda}
$$

As such, this is what we're going to have from the $$P(X = k)$$.

$$
P(X = k) = \frac{\lambda^k}{k!} \times (1 - 0) (1 - 0) ... (1 - 0) \times e^{- \lambda} \times (1 - 0)^{-k} \\
P(X = k) = \frac{\lambda^k}{k!} \times 1 \times e^{- \lambda} \times 1
$$

Therefore, we're going to get the Poisson function as the final result.

$$
P(X = k) = \frac{\lambda^k e^{- \lambda}}{k!}
$$

# Binomial to Infinity

After watching [the video](https://www.youtube.com/watch?v=ceOwlHnVCqo) I tried doing the derivation by myself to confirm that it's indeed related, and I started to wonder if Poisson distribution is simply a very specific case of binomial distribution where the number of trials $$n \to \infty$$ as in Poisson process we consider the trials to keep happening at every moment of time during the time period. I then stumbled upon [this article](https://medium.com/@andrew.chamberlain/deriving-the-poisson-distribution-from-the-binomial-distribution-840cc1668239) by Dr Andrew Chamberlain and [this article](https://www.vosesoftware.com/riskwiki/DerivingthePoissondistributionfromtheBinomial.php) from Vose Software which confirmed my guess.

It's funny in a way, that I got interested in trying to check out how Poisson distribution's probability function is constructed because it seems like a very complicated function due to its use of $$e$$, exponentiation, and factorial in the formula in a way that's not very obvious at the first glance where all of those complicated operations come from. Also, I didn't really think of it as a case of a binomial distribution due to the number of trials not being defined in the formula and the Poisson process itself doesn't seem to expect any number of trials to be done when waiting for the hit occurences to happen.

With the trial $$n \to \infty$$, we can consider the trials to always happen in every single moment we pass during the time period of the Poisson process. We only consider the occurences of the event to be the number of hits we have from all of the trials $$n \to \infty$$ we have. As the number of trials is considered to be approaching infinity and $$\lambda = np$$, we're going to have the probability $$p \to 0$$ if the rate of occurences $$\lambda$$ for the defined time period remains constant.

Yet, Poisson distribution is not really to be translated as a distribution for rare events despite of $$p \to 0$$ assumption on the formula. Rephrasing [the article](https://www.vosesoftware.com/riskwiki/DerivingthePoissondistributionfromtheBinomial.php) from Vose Software, with Poisson distribution we're modeling the problem around the rate of occurence of the event $$\lambda$$ instead of the number of trials $$n$$ and the probability of hit $$p$$. Having the probability of hit $$p \to 0$$ is simply a balancing force in order to keep the $$\lambda$$ constant when we're assuming the number of trials $$n \to \infty$$.

# References

[e - Euler's number](https://www.mathsisfun.com/numbers/e-eulers-number.html)

[The Poisson Distribution and Poisson Process Explained](https://towardsdatascience.com/the-poisson-distribution-and-poisson-process-explained-4e2cb17d459)

[Proof that the Binomial Distribution tends to the Poisson Distribution](https://www.youtube.com/watch?v=ceOwlHnVCqo)

[Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution)

[Binomial coefficient](https://en.wikipedia.org/wiki/Binomial_coefficient)

[Deriving the Poisson Distribution from the Binomial Distribution](https://medium.com/@andrew.chamberlain/deriving-the-poisson-distribution-from-the-binomial-distribution-840cc1668239)

[Deriving the Poisson distribution from the Binomial](https://www.vosesoftware.com/riskwiki/DerivingthePoissondistributionfromtheBinomial.php)
