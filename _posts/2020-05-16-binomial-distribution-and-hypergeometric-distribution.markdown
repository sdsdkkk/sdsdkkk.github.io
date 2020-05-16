---
layout: content
title: "Binomial Distribution and Hypergeometric Distribution"
date: 2020-05-16 00:00:00
description: Diving into the mathematical relation between the two distributions
---

Both binomial distribution and hypergeometric distribution is used to measure the probability of having $$k$$ successes in $$n$$ draws from a population containing $$N$$ objects, with $$K$$ objects are considered successes if drawn. The difference is that binomial distribution assumes the drawn object is replaced after every draw, while hypergeometric distribution assumes no replacement.

[Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution) is formulated as follows.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) p^k (1 - p)^{n - k}
$$

While [hypergeometric distribution](https://en.wikipedia.org/wiki/Hypergeometric_distribution) is formulated as follows.

$$
P(X = k) = \frac{
\left(
  \begin{matrix}
    K \\
    k \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    N - K \\
    n - k \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    N \\
    n \\
  \end{matrix}
\right)
}
$$

As I [previously learned](/2020/05/deriving-poisson-probability-function-from-binomial-distribution.html) that Poisson distribution is derived from binomial distribution, I'd like to see the mathematical relation between binomial distribution and hypergeometric distribution as the use case for both are quite similar.

# Demonstration

Let's see how the distributions are used to calculate probabilities for the cases they're made for.

Suppose we have a jar containing 100 balls, where 25 of them are black and the rest are white. If we draw 20 balls at random without replacement, the following is the probability for us to draw exactly 10 black balls in the draw using hypergeometric distribution formula.

$$
P(X = k) = \frac{
\left(
  \begin{matrix}
    K \\
    k \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    N - K \\
    n - k \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    N \\
    n \\
  \end{matrix}
\right)
} \\
P(X = 10) = \frac{
\left(
  \begin{matrix}
    25 \\
    10 \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    100 - 25 \\
    20 - 10 \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    100 \\
    20 \\
  \end{matrix}
\right)
} \\
P(X = 10) = \frac{
\left(
  \begin{matrix}
    25 \\
    10 \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    75 \\
    10 \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    100 \\
    20 \\
  \end{matrix}
\right)
} \\
P(X = 10) = \frac{\frac{25!}{(25 - 10)! 10!} \frac{75!}{(75 - 10)! 10!}}{\frac{100!}{(100 - 20)! 20!}} \\
P(X = 10) = \frac{\frac{25!}{15! 10!} \frac{75!}{65! 10!}}{\frac{100!}{80! 20!}}
$$

To make it quicker I'm using Matlab to calculate the factorials and get the final result. Here it is.

$$
P(X = 10) = 0.0050553
$$

If every time we draw a ball we put it back again in the jar before the next draw, we're going to have replacement. In this case, binomial distribution is used instead because the number of balls inside the jar doesn't change for every draw as assumed in the hypergeometric distribution. Since the number of balls $$N = 100$$ and the number of black balls $$K = 25$$, the probability of pulling a black ball is $$p = \frac{25}{100} = 0.25$$ and the probability $$p$$ will be constant for each draw.

Suppose we're drawing 20 balls, the probability of getting 10 black balls out of the 20 balls we draw is as follows.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) p^k (1 - p)^{n - k} \\
P(X = 10) = \left(
  \begin{matrix}
    20 \\
    10 \\
  \end{matrix}
\right) 0.25^{10} (1 - 0.25)^{20 - 10} \\
P(X = 10) = \frac{20!}{(20 - 10)! 10!} 0.25^{10} (0.75)^{10} \\
P(X = 10) = \frac{20!}{10! 10!} 0.25^{10} (0.75)^{10}
$$

So we're going to get the result as follows.

$$
P(X = 10) = 0.0099223
$$

# Deriving Binomial from Hypergeometric

According to this [study material](http://faculty.washington.edu/fm1/394/Materials/Hypergeometric.pdf) from the University of Washington, binomial distribution is hypergeometric distribution where $$N$$ and $$K$$ will always have a replacement after drawn, hence $$N, K \to \infty$$.

$$
P(X = k) = \frac{
\left(
  \begin{matrix}
    K \\
    k \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    N - K \\
    n - k \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    N \\
    n \\
  \end{matrix}
\right)
} \\
P(X = k) = \frac{K!}{(K - k)! k!} \frac{(N - K)!}{(n - k)! (N - K - n + k)!} \frac{n! (N - n)!}{N!}
$$

Considering that $$\frac{n!}{(n - k)! k!} = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right)$$, we can rearrange the equation as follows.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) \frac{K!}{(K - k)!} \frac{(N - K)!}{(N - K - n + k)!} \frac{(N - n)!}{N!}
$$

Each fractions in the equation can be transformed. Here's for $$\frac{K!}{(K - k)!}$$.

$$
\frac{K!}{(K - k)!} = \frac{K (K - 1) (K - 2) ... (K - k + 1) (K -k)!}{(K - k)!} \\
\frac{K!}{(K - k)!} =  K (K - 1) (K - 2) ... (K - k + 1)
$$

Here's for $$\frac{(N - K)!}{(N - K - n + k)!}$$.

$$
\frac{(N - K)!}{(N - K - n + k)!} = \frac{(N - K)!}{(N - K - (n - k))!} \\
\frac{(N - K)!}{(N - K - n + k)!} = \frac{(N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1) (N - K - (n - k))!}{(N - K - (n - k))!} \\
\frac{(N - K)!}{(N - K - n + k)!} = (N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)
$$

Here's for $$\frac{(N - n)!}{N!}$$.

$$
\frac{(N - n)!}{N!} = \frac{(N - n)!}{N (N - 1) (N - 2) ... (N - n + 1) (N - n)!} \\
\frac{(N - n)!}{N!} = \frac{1}{N (N - 1) (N - 2) ... (N - n + 1)}
$$

Substituting those to the probability equation, we get the equation as follows.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) \frac{K (K - 1) (K - 2) ... (K - k + 1) \cdot (N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)}{N (N - 1) (N - 2) ... (N - n + 1)}
$$

Since $$k \leq n$$, we can transform the equation into this.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) \frac{K (K - 1) (K - 2) ... (K - k + 1) \cdot (N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)}{N (N - 1) (N - 2) ... (N - k + 1) \cdot (N - k) (N - k - 1) (N - k - 2) ... (N - n + 1)} \\
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) \frac{K (K - 1) (K - 2) ... (K - k + 1)}{N (N - 1) (N - 2) ... (N - k + 1)} \frac{(N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)}{(N - k) (N - k - 1) (N - k - 2) ... (N - n + 1)}
$$

Now we apply the limit $$N, K \to \infty$$ to both fractions. For the first one, we're basically performing $$x (x - 1) (x - 2) ... (x - (k - 1))$$ calculation on $$K$$ and $$N$$. If we assume $$N, K \to \infty$$ then both $$K$$ and $$N$$ remains approximately the same even after having values up to $$k - 1$$ subtracted from them. Therefore, we get the approximation as follows.

$$
\lim_{N, K \to \infty} \frac{K (K - 1) (K - 2) ... (K - k + 1)}{N (N - 1) (N - 2) ... (N - k + 1)} \approx \frac{K^k}{N^k} = p^k
$$

And now for the second fraction. If we apply the limit $$N, K \to \infty$$ on $$(N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)$$ it's quite obvious that we can approximate it as $$(N - K)^{n - k}$$ given that we're repeating the process up to $$n - k$$ times.

For $$(N - k) (N - k - 1) (N - k - 2) ... (N - n + 1)$$, it's originally supposed to be $$N (N - 1) (N - 2) ... (N - n + 1)$$ which is approximately $$N^n$$ if the limit is applied. But we splitted it into $$N (N - 1) (N - 2) ... (N - k + 1)$$ and $$(N - k) (N - k - 1) (N - k - 2) ... (N - n + 1)$$. Given $$N, K \to \infty$$ we can approximate $$N (N - 1) (N - 2) ... (N - k + 1) \approx N^k$$, therefore $$(N - k) (N - k - 1) (N - k - 2) ... (N - n + 1) \approx N^{n - k}$$.

So the second fraction can be approximated as follows.

$$
\lim_{N, K \to \infty} \frac{(N - K) (N - K - 1) (N - K - 2) ... (N - K - (n - k) + 1)}{(N - k) (N - k - 1) (N - k - 2) ... (N - n + 1)} \approx \frac{(N - K)^{n - k}}{N^{n - k}}
$$

We can transform $$\frac{(N - K)^{n - k}}{N^{n - k}}$$ into an equation incorporating the probability $$p$$.

$$
\frac{(N - K)^{n - k}}{N^{n - k}} = (\frac{N - K}{N})^{n - k} = (1 - \frac{K}{N})^{n - k} = (1 - p)^{n - k}
$$

Therefore, applying the limit approximation to the probability equation will get us the binomial distribution formula.

$$
P(X = k) = \left(
  \begin{matrix}
    n \\
    k \\
  \end{matrix}
\right) p^k (1 - p)^{n - k}
$$

Having a replacement for every draw allows us to not run out of object to be drawn, therefore the total number of objects $$N \to \infty$$. Since the replacement would be exactly the same with the object we've drawn, we also wouldn't run out of the objects required for the draw to be considered a success as the total number of success objects $$K \to \infty$$.

The probability of drawing the correct object formulated as $$p = \frac{K}{N}$$ will be constant, since $$K$$ and $$N$$ stays the same regardless of how many times an object is drawn from the population due to having replacement in place.

# References

[Binomial distribution](https://en.wikipedia.org/wiki/Binomial_distribution)

[Hypergeometric distribution](https://en.wikipedia.org/wiki/Hypergeometric_distribution)

[Deriving Poisson Probability Function from Binomial Distribution](/2020/05/deriving-poisson-probability-function-from-binomial-distribution.html)

[The Hypergeometric Distribution](http://faculty.washington.edu/fm1/394/Materials/Hypergeometric.pdf)
