---
layout: post
title: "Moore-Penrose Inverse"
date: 2020-09-09 00:00:00
description: The generalization of matrix inverse
tags: LinearAlgebra
---

I got introduced to the [Moore-Penrose inverse](https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse), also known as pseudoinverse or generalized inverse, in an internal sharing session at my company's engineering meeting. Since I haven't really heard about it before and it seems to be quite useful in the context of work of our data science team, I'd like to look into it.

The Moore-Penrose inverse of a matrix $$A$$ is denoted as $$A^{+}$$, instead of the usual $$A^{-1}$$ notation used to represent the inverse of matrix $$A$$.

For matrix inverse, these rules regarding the identity matrix should apply.

$$
A A^{-1} = I \\
A^{-1} A = I
$$

With Moore-Penrose inverse, the product between $$A$$ and $$A^{+}$$ and vice versa is approximately equal to the identity matrix instead of being exactly equal.

$$
A A^{+} \approx I \\
A^{+} A \approx I
$$

# Identity Matrix

An [identity matrix](https://www.onlinemathlearning.com/identity-matrix.html) is a matrix $$I$$ where the product of $$I$$ and any matrix $$A$$ in any order would result to $$A$$.

An identity matrix only has the values of ones and zeroes, where the ones form a diagonal line from the top left element down to the bottom right element. The other elements are zeroes.

Identity matrix for a 2x2 matrix is as follows.

$$
\left[
    \begin{matrix}
    1 & 0 \\
    0 & 1
    \end{matrix}
\right]
$$

The following is the identity for a 3x3 matrix.

$$
\left[
    \begin{matrix}
    1 & 0 & 0 \\
    0 & 1 & 0 \\
    0 & 0 & 1
    \end{matrix}
\right]
$$

This is for a 4x4 matrix.

$$
\left[
    \begin{matrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 1
    \end{matrix}
\right]
$$

And so on.

# Matrix Inverse

A matrix can be inverted only if its dimensions are $$m$$x$$n$$ where $$m = n$$ and $$m > 1$$. For common matrix calculation of 2x2 and 3x3 matrix, Wolfram Mathworld has [an article](https://mathworld.wolfram.com/MatrixInverse.html) showing how to invert the matrix.

For a 2x2 matrix, we can use this simple method to invert it. Suppose we have a matrix $$A = \left[
    \begin{matrix}
    a & b \\
    c & d
    \end{matrix}
\right]$$, the inversion of $$A$$ is as follows.

$$
A^{-1} = \frac{1}{\lvert A \rvert} \left[
    \begin{matrix}
    d & -b \\
    -c & a
    \end{matrix}
\right]
$$

Where $$\lvert A \rvert$$ is the [determinant](https://www.toppr.com/guides/maths/determinants/determinant-of-a-matrix/) of the matrix $$A$$. For a 2x2 matrix $$A = \left[
    \begin{matrix}
    a & b \\
    c & d
    \end{matrix}
\right]$$, the determinant can be calculated as follows.

$$
\lvert A \rvert = ad - bc
$$

For a 3x3 matrix, the determinant calculation is a bit more complicated as we can see [here](https://d1whtlypfis84e.cloudfront.net/guides/wp-content/uploads/2018/08/26013106/Merging-Result-min.pdf).

The determinant of a 3x3 matrix $$A = \left[
    \begin{matrix}
    a & b & c \\
    d & e & f \\
    g & h & i
    \end{matrix}
\right]$$ would be as follows.

$$
\lvert A \rvert = a(ei - fh) - b(di - fg) + c(dh - eg)
$$

And the inverse of the 3x3 matrix $$A = \left[
    \begin{matrix}
    a & b & c \\
    d & e & f \\
    g & h & i
    \end{matrix}
\right]$$ can be calculated as follows according to the determinants of matrices described in the 3x3 matrix inverse section on [this article](https://mathworld.wolfram.com/MatrixInverse.html).

$$
A^{-1} = \frac{1}{\lvert A \rvert} \left[
    \begin{matrix}
    (ei - fh) & (ch - bi) & (bf - ce) \\
    (fg - di) & (ai - cg) & (cd - af) \\
    (dh - ei) & (bg - ah) & (ae - bd)
    \end{matrix}
\right]
$$

As we can see, adding the dimension from 2x2 to 3x3 increases the complexity of the calculation significantly. A simpler general method we can use to calculate the inverse of a $$n$$x$$n$$ matrix is by using [Gauss-Jordan elimination](https://mathworld.wolfram.com/Gauss-JordanElimination.html).

The idea of Gauss-Jordan elimination is to transform a matrix $$A$$ with the dimension $$n$$x$$n$$ into the identity matrix for $$n$$x$$n$$ by performing eliminations, but the same process applied to the matrix $$A$$ is also applied to its identity matrix.

$$
\left[
    \begin{matrix}
    A & I
    \end{matrix}
\right] \to \left[
    \begin{matrix}
    I & A^{-1}
    \end{matrix}
\right]
$$

The process transforms the matrix $$A$$ into its identity matrix $$I$$, while also transforms the identity matrix $$I$$ into the inverse matrix $$A^{-1}$$.

A demonstration of the Gauss-Jordan elimination to find the inverse of a matrix can be seen in [this video](https://www.youtube.com/watch?v=Fg7_mv3izR0) by The Organic Chemistry Tutor on YouTube.

# Moore-Penrose Inverse

As we see in the preious section on matrix inverse, we need the matrix's dimension to have the same size on width and height for the matrix to be invertible. Basically the matrix need to be square-shaped. Moore-Penrose inverse is a method to extend the matrix inversion method so it can be used to non-square-shaped rectangular matrix. Since it's also called as pseudoinverse, starting from this point we're going refer to the Moore-Penrose inverse as the pseudoinverse to make it shorter.

The pseudoinverse of a matrix $$A$$ can be calculated as follows.

$$
A^{+} = (A^{T} A)^{-1} A^{T}
$$

[This video](https://www.youtube.com/watch?v=pTUfUjIQjoE) by CBlissMath on YouTube explains how the pseudoinverse can be used to help solve linear equations where we have a lot more equations than the unknown variables.

Suppose that we have some linear equations that we model as a matrix operation as follows.

$$
\vec{y} = A \vec{x}
$$

Where $$\vec{x}$$ is the matrix of unknown variables within the equation and $$\vec{y}$$ is the matrix of the calculation results of the individual equation, and $$A$$ is the matrix of multipliers of the variables in $$\vec{x}$$.

We can find $$\vec{x}$$ by performing these steps. First, we transform the matrices on both sides of the matrix equation with the pseudoinverse of $$A$$.

$$
\vec{y} = A \vec{x} \\
A^{+} \vec{y} \approx A^{+} A \vec{x}
$$

Since $$A^{+} A \approx I$$, now we have the matrix equation as follows.

$$
A^{+} \vec{y} \approx I \vec{x}
$$

Since $$I \vec{x} = \vec{x}$$, the equation becomes as follows.

$$
A^{+} \vec{y} \approx \vec{x} \\
\vec{x} \approx A^{+} \vec{y}
$$

We can simply use the regular inverse instead of the pseudoinverse if the matrix $$A$$ is invertible. But in the case where the matrix $$A$$ is not invertible, we can use the pseudoinverse to calculate the approximation of the inverse.

[This study material](http://www.sci.utah.edu/~gerig/CS6640-F2012/Materials/pseudoinverse-cis61009sl10.pdf) from the Scientific Computing and Imaging Institute of the University of Utah provides the proof that the pseudoinverse provides the optimal solution to the least-squares problem in its calculation.

# Extra Notes

Calculating matrix operations by hand is pretty cumbersome and prone to human error. Use tools such as Matlab to help calculating matrix operations whenever possible for more reliable results.

# References

[Moore-Penrose inverse](https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse)

[Identity Matrices](https://www.onlinemathlearning.com/identity-matrix.html)

[Matrix Inverse](https://mathworld.wolfram.com/MatrixInverse.html)

[Determinant of a Matrix](https://www.toppr.com/guides/maths/determinants/determinant-of-a-matrix/)

[Determinants](https://d1whtlypfis84e.cloudfront.net/guides/wp-content/uploads/2018/08/26013106/Merging-Result-min.pdf)

[Gauss-Jordan Elimination](https://mathworld.wolfram.com/Gauss-JordanElimination.html)

[Inverse of a 3x3 Matrix](https://www.youtube.com/watch?v=Fg7_mv3izR0)

[Introduction to the pseudo-inverse](https://www.youtube.com/watch?v=pTUfUjIQjoE)

[Least Squares, Pseudo-Inverses, PCA & SVD](http://www.sci.utah.edu/~gerig/CS6640-F2012/Materials/pseudoinverse-cis61009sl10.pdf)
