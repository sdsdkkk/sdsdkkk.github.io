---
layout: content
title: "Multiple Integral and Partial Derivative"
date: 2020-06-09 00:00:00
description: Scratching a bit of the surface of multivariable calculus
---

In the effort of refreshing my [calculus](/2020/06/integral-and-differential-calculus.html) knowledge, I ended up exploring [multivariable calculus](https://www.wyzant.com/resources/lessons/math/calculus/multivariable_vectors). To be honest, I don't remember ever learning anything on this topic before.

After poking around at some materials regarding multivariable calculus I found available on the Internet, it seems that the basic concepts are exactly the same. But since there are added dimensions into the calculation, there are some extra steps that needs to be made.

# Multiple Integral

To do integral calculus on a multivariable function, we need to perform the integral calculation according to the number of the variables involved. It's usually called double integral or triple integral if we need to integrate the function over two or three variables, but we can go with even more variables than that with this [multiple integral](https://en.wikipedia.org/wiki/Multiple_integral) method.

$$
\int \int f(x, y) dx\ dy \\
\int \int \int f(x, y, z) dx\ dy\ dz
$$

Suppose we have a three-dimensional graph for the following function with two variables.

$$
z = f(x, y)
$$

The volume of the area under the curved plane $$z$$ where $$a \leq x \leq b$$ and $$c \leq y \leq d$$ can be calculated as follows.

$$
\int_{c}^{d} \int_{a}^{b} f(x, y) dx\ dy
$$

Where $$\int_{a}^{b} f(x, y) dx$$ is the function to calculate the area of the slice where $$a \leq x \leq b$$ given $$y$$. $$\int_{c}^{d} \int_{a}^{b} f(x, y) dx\ dy$$ is the total volume under the curved plane given that $$a \leq x \leq b$$ and $$c \leq y \leq d$$.

We can do the integration in any ordering.

$$
\int_{c}^{d} \int_{a}^{b} f(x, y) dx\ dy = \int_{a}^{b} \int_{c}^{d} f(x, y) dy\ dx
$$

# Partial Derivative and Gradient

We can derive a multivariable function [partially](https://en.wikipedia.org/wiki/Partial_derivative) to one of the variables to get the slope of the function in regard of a variable at a certain point in the graph.

$$
z = f(x, y) \\
\frac{\partial z}{\partial x} = \frac{\partial f(x, y)}{\partial x} \\
\frac{\partial z}{\partial y} = \frac{\partial f(x, y)}{\partial y}
$$

From the partial derivatives, we can construct the [gradient](https://en.wikipedia.org/wiki/Gradient) is as follows.

$$
\nabla f(x, y) = \left[
    \begin{matrix}
    \frac{\partial f}{\partial x}(x, y) \\
    \frac{\partial f}{\partial y}(x, y)
    \end{matrix}
  \right]
$$

The gradient is a [vector](https://en.wikipedia.org/wiki/Vector_(mathematics_and_physics)) pointing at the steepest ascending slope at the point. It's a bit difficult to visualize using only lines on two-dimensional plane, so [this article](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/partial-derivative-and-gradient-articles/a/the-gradient) on Khan Academy might do a better job explaining it.

The gradient can be used to calculate the [directional derivative](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/partial-derivative-and-gradient-articles/a/directional-derivative-introduction) of the function.

$$
\nabla_{\vec{v}} f(x, y) = v_1 \frac{\partial f}{\partial x}(x, y) + v_2 \frac{\partial f}{\partial y}(x, y)
$$

# Extra Notes

So far I see the differential parts in multivariable calculus as more complex to understand compared to the integral parts. This is due to the integral being pretty much the same as the single-variable calculus, simply measuring the area under the curve but just with more dimensions to handle which translates to more integral operations to be done.

As for the differential, we can have the partial derivatives of the function to calculate the slope at one point relative to an axis in the graph. But that's not the whole calculation yet, as we've just calculated one-dimensional slopes of a multi-dimensional function. We need to use the gradient to combine the partial derivatives into a vector that points to the direction of the steepest slope at a certain point.

At this point I still haven't really understood about the vector derivative and I might need to look deeper into it. But multivariable calculus is sure more complex that what I initially thought as simply calculating the area under a curve and finding the slopes of the curve. Well, that part is true regarding multiple integral and partial derivative, but I didn't have much knowledge of [vector fields](https://www.khanacademy.org/math/multivariable-calculus/thinking-about-multivariable-function/ways-to-represent-multivariable-functions/a/vector-fields) and vector derivative when I first jumped into it.

I definitely underestimated the complexity of multivariable calculus, and I was correct that I've never learned about it before. Also, there's definitely more to even just the [multivariable integrals](https://mathinsight.org/integrals_multivariable_calculus_summary) than what I've learned in the last few days.

# References

[Integral and Differential Calculus](/2020/06/integral-and-differential-calculus.html)

[Multivariable Calculus](https://www.wyzant.com/resources/lessons/math/calculus/multivariable_vectors)

[Multiple integral](https://en.wikipedia.org/wiki/Multiple_integral)

[Partial derivative](https://en.wikipedia.org/wiki/Partial_derivative)

[Gradient](https://en.wikipedia.org/wiki/Gradient)

[Vector (mathematics and physics)](https://en.wikipedia.org/wiki/Vector_(mathematics_and_physics))

[The Gradient](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/partial-derivative-and-gradient-articles/a/the-gradient)

[Directional derivatives (introduction)](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/partial-derivative-and-gradient-articles/a/directional-derivative-introduction)

[Vector fields](https://www.khanacademy.org/math/multivariable-calculus/thinking-about-multivariable-function/ways-to-represent-multivariable-functions/a/vector-fields)

[The integrals of multivariable calculus](https://mathinsight.org/integrals_multivariable_calculus_summary)
