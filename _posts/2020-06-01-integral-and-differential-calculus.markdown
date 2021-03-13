---
layout: post
title: "Integral and Differential Calculus"
date: 2020-06-01 00:00:00
description: A note regarding the basic concepts of calculus
tags: Calculus
---

This article serves as a note for me regarding [integral](https://en.wikipedia.org/wiki/Integral) and [differential calculus](https://en.wikipedia.org/wiki/Differential_calculus). The integral operation itself has some properties which is not covered in this article, but is available [here](https://tutorial.math.lamar.edu/Classes/Calci/ProofIntProp.aspx). For the derivative there's a note on the properties [here](https://amsi.org.au/ESA_Senior_Years/SeniorTopic3/3b/3b_2content_6.html).

Suppose we have a function $$f$$ where $$y = f(x)$$ as pictured below.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
  \end{tikzpicture}
</script>

In order to calculate the area under the curve in the range $$a \leq x \leq b$$, which is defined as within the interval $$[a, b]$$, we can use integral of $$f(x)$$ as follows.

$$
\int_{a}^{b} f(x) dx
$$

Given that $$F(x) = \int f(x) dx$$, the area under the curve can be calculated as follows.

$$
\int_{a}^{b} f(x) dx = [F(x)]_{a}^{b} = F(b) - F(a)
$$

Aside from calculating the area under the curve, we can also calculate the slope of the tangent line of the curve at one point of $$x$$ by using [derivative](https://en.wikipedia.org/wiki/Derivative) to get the differentiation.

$$
f'(x) = \frac{dy}{dx}
$$

The derivative is the opposite of integral, so with $$F(x) = \int f(x) dx$$ we get $$F'(x) = f(x)$$. Therefore, we can say that $$F(x)$$ is the antiderivative of $$f(x)$$.

# Differential Calculus

Differential calculus is used to measure the rate of change at a certain point on a curve. This is the graph from the beginning of this article.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
  \end{tikzpicture}
</script>

We can calculate the change from point $$(a, f(a))$$ to point $$(b, f(b))$$ by calculating the [slope](https://en.wikipedia.org/wiki/Slope) of a straight line from point $$(a, f(a))$$ to $$(b, f(b))$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
    \draw [blue] (4,4.625) -- (12,6.6) node[pos=-0.03] {v};
  \end{tikzpicture}
</script>

The line $$v$$ (the blue line) is the line from $$(a, f(a))$$ to $$(b, f(b))$$. The slope $$m$$ can be calculated as follows.

$$
m = \frac{\Delta y}{\Delta x} = \frac{y_2 - y_1}{x_2 = x_1} = \frac{f(b) - f(a)}{b - a}
$$

Notice that it's similar to the formula for derivative shown before.

$$
f'(x) = \frac{dy}{dx}
$$

The difference is that the function from the derivative calculates for the rate of change at a certain point instead of the rate of change between two points. Suppose we want to find the slope between the points $$(b, f(b))$$ and $$(b + h, f(b + h))$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
    \draw (13,0) -- (13,6.65) node[pos=-0.03] {b + h};
    \draw [blue] (12,6.6) -- (13,6.65) node[pos=-0.03] {v};
  \end{tikzpicture}
</script>

The line $$v$$ is the change between $$(b, f(b))$$ and $$(b + h, f(b + h))$$, positioned as the secant line of the curve betwen the two points. To get the tangential line on $$x = b$$, we need to find the slope between the point $$(b, f(b))$$ and $$(b + h, f(b + h))$$ where $$h \to 0$$.

$$
m = \frac{\Delta y}{\Delta x} = \lim_{h \to 0} \frac{f(b + h) - f(b)}{(b + h) - b} = \lim_{h \to 0} \frac{f(b + h) - f(b)}{h}
$$

If we apply the limit $$h \to 0$$ to the slope formula we're going to get the derivative of the function $$f(x)$$.

$$
f'(x) = \frac{dx}{dy} = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h}
$$

# Integral Calculus

I'm taking some references from [this video](https://www.youtube.com/watch?v=NLU9U8-wJrM) by Dave Farina. Let's start with the same graph as we see in the beginning of this article.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
  \end{tikzpicture}
</script>

We can calculate the area under the curve by drawing a rectangle with approximately the same size of the area under the curve.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,5.6125) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
    \draw (4,5.6125) -- (12,5.6125);
  \end{tikzpicture}
</script>

The rectangle's area of coverage doesn't exactly match the area under the curve we're trying to calculate, but the area of the rectangle should work as an approximation of the area under the curve.

Suppose we pick $$x$$ in the interval $$[a, b]$$, $$x$$ is defined as $$x = a + h$$ and $$x \leq b$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
    \draw (8,0) -- (8,5.575) node[pos=-0.03] {a+h};
  \end{tikzpicture}
</script>

We can draw two rectangles now.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,5.025) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
    \draw (8,0) -- (8,6.05) node[pos=-0.03] {a+h};
    \draw (4,5.025) -- (8,5.025);
    \draw (8,6.05) -- (12,6.05);
  \end{tikzpicture}
</script>

We should be able to get a better approximation by using more rectangles with less width in terms of $$\Delta x$$. To get the most number of rectangles, we need to make the rectangle as thin as possible with $$\Delta x \to 0$$ converge to the curve's area and calculate the sum of all $$n$$ rectangles' area to get the approximation.

$$
A_{total} = \sum_{i = 0}^{n} f(t_i) (x_{i + 1} - x_i)
$$

Where $$f(t_i)$$ is the height of the rectangle and $$\Delta x = x_{i + 1} - x_i$$ is the width. This is the basic idea of [Riemann integral](https://en.wikipedia.org/wiki/Riemann_integral).

[Newton](https://en.wikipedia.org/wiki/Isaac_Newton) and [Leibniz](https://en.wikipedia.org/wiki/Gottfried_Wilhelm_Leibniz) later refined the method of calculating the integral by using the antidifferentiation of the function $$f(x)$$ to calculate the area under the curve within a certain interval.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,10) node[pos=1.025] {y};
    \draw [->] (0,0) -- (17,0) node[pos=1.025] {x};
    \draw (0,5) to[out=-20] (17,5) node {f(x)};
    \draw (4,0) -- (4,4.625) node[pos=-0.03] {a};
    \draw (12,0) -- (12,6.6) node[pos=-0.03] {b};
  \end{tikzpicture}
</script>

$$
\int_{a}^{b} f(x) dx = [F(x)]_{a}^{b} = F(b) - F(a)
$$

$$F(x)$$ is the antiderivative of $$f(x)$$, therefore $$F'(x) = f(x)$$ and $$F(x) = \int f(x) dx$$. This is defined as the [fundamental theorem of calculus](https://en.wikipedia.org/wiki/Fundamental_theorem_of_calculus).

# References

[Integral](https://en.wikipedia.org/wiki/Integral)

[Differential calculus](https://en.wikipedia.org/wiki/Differential_calculus)

[Calculus I - Proof of Various Integral Properties](https://tutorial.math.lamar.edu/Classes/Calci/ProofIntProp.aspx)

[Properties of the derivative](https://amsi.org.au/ESA_Senior_Years/SeniorTopic3/3b/3b_2content_6.html)

[Derivative](https://en.wikipedia.org/wiki/Derivative)

[Slope](https://en.wikipedia.org/wiki/Slope)

[The Fundamental Theorem of Calculus: Redefining Integration](https://www.youtube.com/watch?v=NLU9U8-wJrM)

[Riemann integral](https://en.wikipedia.org/wiki/Riemann_integral)

[Isaac Newton](https://en.wikipedia.org/wiki/Isaac_Newton)

[Gottfried Wilhelm Leibniz](https://en.wikipedia.org/wiki/Gottfried_Wilhelm_Leibniz)

[Fundamental theorem of calculus](https://en.wikipedia.org/wiki/Fundamental_theorem_of_calculus)
