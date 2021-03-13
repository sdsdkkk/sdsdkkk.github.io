---
layout: post
title: "Riemann Zeta Function"
date: 2020-08-21 00:00:00
description: The zeta function and prime numbers
tags: NumberTheory
---

The [zeta function](https://en.wikipedia.org/wiki/Riemann_zeta_function), which is commonly known as the Riemann zeta function, is defined as follows.

$$
\zeta (s) = \sum_{n = 1}^{\infty} \frac{1}{n^s}
$$

The zeta function's [Dirichlet series](https://en.wikipedia.org/wiki/Dirichlet_series) will converge when the real part of $$s$$ is greater than 1. This is under the assumption that $$s$$ is a complex number, which can have both real and imaginary components to it.

While the zeta function itself wasn't discovered by Riemann, it's most commonly associated to Riemann due to the [Riemann hypothesis](https://en.wikipedia.org/wiki/Riemann_hypothesis).

# Euler Product Formula

Leonhard Euler proved the relation of the prime numbers and the zeta function as follows, given that $$p \in P$$ where $$P$$ is a set containing all prime numbers greater than 1.

$$
\zeta (s) = \sum_{n=1}^{\infty} \frac{1}{n^s} = \prod_{p} \frac{1}{1 - p^{-s}}
$$

Wikipedia has the complete proof [here](https://en.wikipedia.org/wiki/Proof_of_the_Euler_product_formula_for_the_Riemann_zeta_function). But here's how the proof is done.

$$
\zeta (s) = 1 + \frac{1}{2^s} + \frac{1}{3^s} + \frac{1}{4^s} + \frac{1}{5^s} + ... \\
\frac{1}{2^s} \zeta (s) = \frac{1}{2^s} + \frac{1}{4^s} + \frac{1}{6^s} + \frac{1}{8^s} + \frac{1}{10^s} + ...
$$

We can substract $$\frac{1}{2^s} \zeta (s)$$ from $$\zeta (s)$$.

$$
(1 - \frac{1}{2^s}) \zeta (s) = 1 + \frac{1}{3^s} + \frac{1}{5^s} + \frac{1}{7^s} + \frac{1}{9^s} + ...
$$

With $$\frac{1}{3^s} (1 - \frac{1}{2^s}) \zeta (s)$$ we're going to get this.

$$
\frac{1}{3^s} (1 - \frac{1}{2^s}) \zeta (s) = \frac{1}{3^s} + \frac{1}{9^s} + \frac{1}{15^s} + \frac{1}{21^s} + \frac{1}{27^s} + ...
$$

We can subtract it further from $$(1 - \frac{1}{2^s}) \zeta (s)$$.

$$
(1 - \frac{1}{3^s})(1 - \frac{1}{2^s}) \zeta (s) = 1 + \frac{1}{5^s} + \frac{1}{7^s} + \frac{1}{11^s} + \frac{1}{13^s} + \frac{1}{17^s} + ...
$$

Notice that we've eliminated numbers on the right hand side of the equation that have 2 or 3 as their factors. Continuing the process for the factors of 5, 7, and so on will get us to this where $$p \in P$$ and $$P$$ is the set of all prime numbers.

$$
\zeta (s) \prod_{p} 1 - \frac{1}{p^s} = 1 \\
\zeta (s) =  \prod_{p} \frac{1}{1 - \frac{1}{p^s}} \\
\zeta (s) =  \prod_{p} \frac{1}{1 - p^{-s}}
$$

# Riemann Hypothesis

[Bernhard Riemann](https://en.wikipedia.org/wiki/Bernhard_Riemann) extended the zeta function for $$s$$ containing complex numbers. I honestly don't think I've understood the concepts related to the Riemann hypothesis properly yet, but it's an important conjecture regarding the distribution of prime numbers. Ben Riffer-Reinert from the University of Chicago wrote [a paper](https://www.math.uchicago.edu/~may/VIGRE/VIGRE2011/REUPapers/Riffer-Reinert.pdf) on the relation of the zeta function and the prime number theorem here, taking account of trivial and non-trivial zeros of the zeta function as conjectured by Riemann.

According to Riemann, the trivial zeros occur at all negative even integers $$s \in \{ -2, -4, -6, ... \}$$ while the non-trivial zeros are located at some points in the critical strip $$0 < \sigma < 1$$ where $$s \equiv \sigma + it$$ as quoted from [this article](https://mathworld.wolfram.com/RiemannZetaFunctionZeros.html). The Riemann hypothesis asserts that all non-trivial zeros in the critical strip is located on the critical line where the real part of $$s$$ is $$\sigma = \frac{1}{2}$$, which is known to be true for the first $$10^{13}$$ non-trivial zeros found in the critical strip.

# References

[Riemann zeta function](https://en.wikipedia.org/wiki/Riemann_zeta_function)

[Dirichlet series](https://en.wikipedia.org/wiki/Dirichlet_series)

[Riemann hypothesis](https://en.wikipedia.org/wiki/Riemann_hypothesis)

[Proof of the Euler product formula for the Riemann zeta function](https://en.wikipedia.org/wiki/Proof_of_the_Euler_product_formula_for_the_Riemann_zeta_function)

[Bernhard Riemann](https://en.wikipedia.org/wiki/Bernhard_Riemann)

[The Zeta Function and Its Relation to the Prime Number Theorem](https://www.math.uchicago.edu/~may/VIGRE/VIGRE2011/REUPapers/Riffer-Reinert.pdf)

[Riemann Zeta Function Zeros](https://mathworld.wolfram.com/RiemannZetaFunctionZeros.html)
