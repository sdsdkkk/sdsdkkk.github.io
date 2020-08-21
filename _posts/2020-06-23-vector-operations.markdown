---
layout: content
title: "Vector Operations"
date: 2020-06-23 00:00:00
description: Notes and references on basic vector operations
---

[Vectors](https://en.wikipedia.org/wiki/Euclidean_vector) are geometric objects with magnitude and direction. Vectors are spatial in nature, and is commonly used to model movements of objects in 2D or 3D space. Vectors can be represented mathematically by using [matrices](https://en.wikipedia.org/wiki/Matrix_(mathematics)), and there's a particular branch of mathematics called [linear algebra](https://en.wikipedia.org/wiki/Linear_algebra) dedicated to the application of linear equations and linear transformations in vector space.

Suppose a vector $$\vec{a}$$ has the initial point $$A$$ and the terminal point $$B$$, the vector is defined as the path or carrier from point $$A$$ to $$B$$.

This topic needs a lot of visual representations to be explained properly, and I think one of the problems that I had when learning linear algebra more than 10 years ago was that I wasn't taught properly on how to visualize the vector transformations. Hence, I had trouble properly understanding the concepts of spatial linear transformations and only relied on memorizations of the formulas to get by.

Since drawing or animating the visual representations take some effort, I'm going to refer to articles or YouTube videos with the proper representations instead.

# Vector Representation

The following is an image of a vector on a 2D space, from the initial point (0, 0) to the terminal point (a, b).

<script type="text/tikz">
  \begin{tikzpicture}
    \draw [->] (0,0) -- (0,7) node[pos=1.025] {y};
    \draw [->] (0,0) -- (7,0) node[pos=1.025] {x};
    \draw [->] (0,0) -- (5,5) node[pos=1.025] {a, b};
  \end{tikzpicture}
</script>

The point (0, 0) is set as the origin point of the vector in the 2D plane, so the vector $$\vec{u}$$ can be represented using a matrix as follows.

$$
\vec{u} = \left[
    \begin{matrix}
    a \\
    b
    \end{matrix}
  \right]
$$

Note that we can multiply a vector with a scalar value, for example 2.

$$
\vec{u} = 2 \left[
    \begin{matrix}
    a \\
    b
    \end{matrix}
  \right]
  = \left[
    \begin{matrix}
    2a \\
    2b
    \end{matrix}
  \right]
$$

This will result to the vector being twice as long as it originally is.

# Vector Addition and Subtraction

We can perform [additions and subtractions](https://www.varsitytutors.com/hotmath/hotmath_help/topics/adding-and-subtracting-vectors) on a vector. I'm leaving the visualization to the reference link, but with matrices we can perform the following operations.

$$
\vec{u} + \vec{v} = \left[
    \begin{matrix}
    x_{\vec{u}} \\
    y_{\vec{u}}
    \end{matrix}
  \right]
  +
  \left[
    \begin{matrix}
    x_{\vec{v}} \\
    y_{\vec{v}}
    \end{matrix}
  \right]
  = \left[
    \begin{matrix}
      x_{\vec{u}} + x_{\vec{v}} \\
      y_{\vec{u}} + y_{\vec{v}}
    \end{matrix}
  \right]
$$

Vector subtraction is done as a simple matrix subtraction.

$$
\vec{u} + \vec{v} = \left[
    \begin{matrix}
    x_{\vec{u}} \\
    y_{\vec{u}}
    \end{matrix}
  \right]
  -
  \left[
    \begin{matrix}
    x_{\vec{v}} \\
    y_{\vec{v}}
    \end{matrix}
  \right]
  = \left[
    \begin{matrix}
      x_{\vec{u}} - x_{\vec{v}} \\
      y_{\vec{u}} - y_{\vec{v}}
    \end{matrix}
  \right]
$$

# Linear Transformation

Vectors can be transformed in space by using [transformation matrix](https://en.wikipedia.org/wiki/Transformation_matrix) to perform the transformation. [This video](https://www.youtube.com/watch?v=kYB8IZa5AuE) by 3Blue1Brown shows how linear transformation works with a good visualization.

Suppose $$\vec{v}$$ is a vector on a 2D plane, and $$f$$ is a function to transform the vector according to a transformation matrix. Let's say the transformation matrix we're using is the transformation matrix $$T$$ as follows.

$$T = \left[
  \begin{matrix}
    2 & 0 \\
    0 & 2
  \end{matrix}
\right]$$

So the transformation function will be as follows.

$$
f(\vec{v}) = T \vec{v} \\
f(\vec{v}) = \left[
    \begin{matrix}
      2 & 0 \\
      0 & 2
    \end{matrix}
  \right]
  \left[
    \begin{matrix}
      x \\
      y
    \end{matrix}
\right] \\
f(\vec{v}) = 
\left[
  \begin{matrix}
    2 \times x + 0 \times y \\
    0 \times x + 2 \times y
  \end{matrix}
\right] \\
f(\vec{v}) = 
\left[
  \begin{matrix}
    2x \\
    2y
  \end{matrix}
\right]
$$

Suppose we have another transformation function $$g$$ according to the transformation matrix $$U$$ as follows.

$$
U = \left[
  \begin{matrix}
    1 & 2 \\
    2 & 1
  \end{matrix}
\right]
$$

If we transform $$\vec{v}$$ using function $$g$$ and then take the output to be transformed using function $$f$$, we're going to get this result.

$$
f(g(\vec{v})) = T U \vec{v} \\
f(g(\vec{v})) = \left[
  \begin{matrix}
    2 & 0 \\
    0 & 2
  \end{matrix}
\right]
\left[
  \begin{matrix}
    1 & 2 \\
    2 & 1
  \end{matrix}
\right]
\left[
  \begin{matrix}
    x \\
    y
  \end{matrix}
\right] \\
f(g(\vec{v})) = \left[
  \begin{matrix}
    2 & 0 \\
    0 & 2
  \end{matrix}
\right]
\left[
  \begin{matrix}
    1 \times x + 2 \times y \\
    2 \times x + 1 \times y
  \end{matrix}
\right] \\
f(g(\vec{v})) = \left[
  \begin{matrix}
    2 & 0 \\
    0 & 2
  \end{matrix}
\right]
\left[
  \begin{matrix}
    x + 2y \\
    2x + y
  \end{matrix}
\right] \\
f(g(\vec{v})) = \left[
  \begin{matrix}
    2 (x + 2y) + 0 (x + 2y) \\
    0 (2x + y) + 2 (2x + y)
  \end{matrix}
\right] \\
f(g(\vec{v})) = \left[
  \begin{matrix}
    2 (x + 2y) \\
    2 (2x + y)
  \end{matrix}
\right] \\
$$

Note that we perform the matrix operations with the ordering starting from right to left.

# Dot Product and Cross Product

Both dot product and cross product require the [magnitude of vectors](https://mathinsight.org/definition/magnitude_vector) in their calculations. The magnitude is basically the length of the vector.

For a 2D vector $$\vec{a}$$, the magnitude is calculated as follows.

$$
|\vec{a}| = \sqrt{x^2 + y^2}
$$

For a 3D vector, the magnitude is calculated similarly but with an extra dimension.

$$
|\vec{a}| = \sqrt{x^2 + y^2 + z^2}
$$

The [dot product](https://en.wikipedia.org/wiki/Dot_product) can be defined using an algebraic definition as follows where $$n$$ is the number of dimension of the vectors $$\vec{a}$$ and $$\vec{b}$$.

$$
\vec{a} \cdot \vec{b} = \sum_{i = 1}^n a_i b_i
$$

For pairs of 2D and 3D vectors, the calculation is as follows.

$$
\vec{a} \cdot \vec{b} = x_a x_b + y_a y_b \\
\vec{a} \cdot \vec{b} = x_a x_b + y_a y_b + z_a z_b
$$

The dot product can also be defined using the geometric definition as follows, where $$\theta$$ is the angle between vectors $$\vec{a}$$ and $$\vec{b}$$.

$$
\vec{a} \cdot \vec{b} = \vec{b} \cdot \vec{a} = |\vec{a}| |\vec{b}| \cos \theta
$$

The [cross product](https://en.wikipedia.org/wiki/Cross_product) is a bit more complicated.

$$
\vec{a} \times \vec{b} = |\vec{a}| |\vec{b}| \sin \theta\ \vec{n}
$$

The cross product assumes that there are at least three dimensions in the space. The vector $$\vec{n}$$ is a [unit vector](https://en.wikipedia.org/wiki/Unit_vector) normal to $$\vec{a}$$ and $$\vec{b}$$. Note that $$\vec{a} \times \vec{b} \neq \vec{b} \times \vec{a}$$ since $$\vec{n}$$ for $$\vec{b} \times \vec{a}$$ will go to the opposite direction of $$\vec{n}$$ for $$\vec{a} \times \vec{b}$$.

[This article](https://betterexplained.com/articles/cross-product/) on cross product seems to be easier to understand than the Wikipedia article linked before.

# References

[Euclidean vector](https://en.wikipedia.org/wiki/Euclidean_vector)

[Matrix (mathematics)](https://en.wikipedia.org/wiki/Matrix_(mathematics))

[Linear algebra](https://en.wikipedia.org/wiki/Linear_algebra)

[Adding and Subtracting Vectors](https://www.varsitytutors.com/hotmath/hotmath_help/topics/adding-and-subtracting-vectors)

[Transformation matrix](https://en.wikipedia.org/wiki/Transformation_matrix)

[Linear transformations and matrices](https://www.youtube.com/watch?v=kYB8IZa5AuE)

[Magnitude of a vector definition](https://mathinsight.org/definition/magnitude_vector)

[Dot product](https://en.wikipedia.org/wiki/Dot_product)

[Cross product](https://en.wikipedia.org/wiki/Cross_product)

[Unit vector](https://en.wikipedia.org/wiki/Unit_vector)

[Vector Calculus: Understanding the Cross Product](https://betterexplained.com/articles/cross-product/)
