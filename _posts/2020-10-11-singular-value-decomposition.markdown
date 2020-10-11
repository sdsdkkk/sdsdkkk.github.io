---
layout: content
title: "Singular Value Decomposition"
date: 2020-10-11 00:00:00
description: How to compute singular value decomposition of a matrix
---

As described by Professor Gilbert Strang from the Massachusetts Institute of Technology in [this video](https://www.youtube.com/watch?v=mBcLRGuAFUk), every $$m \times n$$ matrix $$A$$ can be factored as follows.

$$
A = U \Sigma V^T
$$

Where $$U$$ is an orthogonal matrix with the dimension $$m \times m$$, $$\Sigma$$ is a diagonal matrix with the dimension $$m \times n$$, and $$V$$ is an orthogonal matrix with the dimension $$n \times n$$.

$$
A^T A = V \Sigma^T U^T U \Sigma V^T \\
A A^T = U \Sigma V^T V \Sigma^T U^T
$$

# Orthogonal Matrix

[Orthogonal matrix](https://en.wikipedia.org/wiki/Orthogonal_matrix) is a square matrix whose transpose is equivalent to its inverse. Suppose we have a matrix $$Q$$, we can say that $$Q$$ is an orthogonal matrix if it fulfills the following condition.

$$
Q^T Q = Q Q^T = I
$$

Where $$I$$ is the identity matrix of $$Q$$, hence $$Q^T = Q^{-1}$$.

The orthogonal matrix $$Q$$ also fits the following equation with vectors $$u$$ and $$v$$.

$$
u \cdot v = (Qu) \cdot (Qv)
$$

Also the following.

$$
v^T v = (Qv)^T (Qv) = v^T Q^T Q v
$$

The determinant of an orthogonal matrix is either -1 or 1, which translates to $$\lvert Q \rvert^2 = 1$$. Any vector transformed by an orthogonal matrix keeps its size the same as the original vector.

[This video](https://www.youtube.com/watch?v=yDwIfYjKEeo) from the Khan Academy gives a good explanation on orthogonal matrix, also [this article](https://www.nagwa.com/en/explainers/476190725258/) from Nagwa.

# Diagonal Matrix

A [diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix) is a matrix whose non-zero elements are lined diagonally starting from the top left. If the matrix is a square matrix, the non-zero elements are lined from the top left until the bottom right as follows.

$$
\left[
  \begin{matrix}
  2 & 0 & 0 \\
  0 & 1 & 0 \\
  0 & 0 & 2
  \end{matrix}
\right]
$$

The following are examples for non-square rectangular matrices.

$$
\left[
  \begin{matrix}
  2 & 0 & 0 \\
  0 & 1 & 0
  \end{matrix}
\right] \\
\left[
  \begin{matrix}
  2 & 0 \\
  0 & 1 \\
  0 & 0
  \end{matrix}
\right]
$$

An [identity matrix](https://en.wikipedia.org/wiki/Identity_matrix) $$I$$ is basically a diagonal square matrix whose non-zero elements are all 1.

# Singular Value Decomposition

As described in the beginning of this article, any $$m \times n$$ matrix can be factored as follows.

$$
A = U \Sigma V^T
$$

We mentioned before that $$U$$ and $$V$$ must be orthogonal matrices with the dimensions $$m \times m$$ and $$n \times n$$ respectively, while $$\Sigma$$ is a diagonal matrix with the dimension $$m \times n$$.

I tried using [this tutorial](https://web.mit.edu/be.400/www/SVD/Singular_Value_Decomposition.htm) from the Massachusetts Institute of Technology as a reference, but their transpose is wrong and when I tried to use the correct transpose for my calculation and got different results I couldn't compare it with the tutorial because the results are different. [This reference](https://www.d.umn.edu/~mhampton/m4326svd_example.pdf) from the University of Minnesota gives another example of singular value decomposition computation I checked, but I ended up taking example from [this YouTube video](https://www.youtube.com/watch?v=Ls2TgGFfZnU) for this article since it's simpler to calculate and the steps are explained pretty clearly.

$$
A = \left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right]
$$

$$A A^T$$ and $$A^T A$$ will have square matrices as the results, from which we can compute the [eigenvectors and eigenvalues](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors).

$$
A A^T = \left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right] \left[
  \begin{matrix}
  1 & 0 & -1 \\
  1 & 1 & 1
  \end{matrix}
\right] = \left[
  \begin{matrix}
  2 & 1 & 0 \\
  1 & 1 & 1 \\
  0 & 1 & 2
  \end{matrix}
\right] \\
A^T A = \left[
  \begin{matrix}
  1 & 0 & -1 \\
  1 & 1 & 1
  \end{matrix}
\right] \left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right] = \left[
  \begin{matrix}
  2 & 0 \\
  0 & 3
  \end{matrix}
\right]
$$

The $$U$$ matrix can be computed from the eigenvectors of $$A A^T$$, while the $$V$$ matrix can be computed from the eigenvectors of $$A^T A$$. To make it easier, I'm using [GNU Octave](https://www.gnu.org/software/octave/) which is compatible with [Matlab](https://www.mathworks.com/products/matlab.html) to help with the computations.

```matlab
A =  [1 1; 0 1; -1 1]

# eigendecomposition of A A^T
# A * A^T = V D W^T = W D V^T
[V, D, W] = eig(A * A')

# eigendecomposition of A A^T
# A^T A = V D W^T = W D V^T
[V, D, W] = eig(A' * A)
```

We start with finding the matrix $$V^T$$ of the $$A = U \Sigma V^T$$ factorization. $$V$$ is an $$n \times n$$ matrix with the eigenvectors of $$A^T A$$ as its columns. From the above Matlab code, we get the eigenvectors of $$A^T A$$ from which we can define the matrix $$V$$.

$$
V = \left[
  \begin{matrix}
  0 & 1 \\
  1 & 0
  \end{matrix}
\right]
$$

We can transpose it to get $$V^T$$, but in this case it's equal to $$V$$.

$$
V^T = \left[
  \begin{matrix}
  0 & 1 \\
  1 & 0
  \end{matrix}
\right]
$$

$$\Sigma$$ is a diagonal matrix which contain the square root of the eigenvalues from the eigenvectors of $$A^T A$$ as the elements in the main diagonal. From the previous Matlab, code, we're going to get eigenvalues $$\lambda_1 = 3$$ and $$\lambda_2 = 2$$.

$$
\Sigma = \left[
  \begin{matrix}
  \sqrt{3} & 0 \\
  0 & \sqrt{2} \\
  0 & 0
  \end{matrix}
\right]
$$

We need to add an extra line of zeros to ensure the dimension of the $$\Sigma$$ matrix is the same as the $$A$$ matrix.

For the matrix $$U$$, we can get the value of each columns by calculating the product of $$u_i = \frac{1}{\sigma_i} A x_i$$ where $$\sigma_i$$ is the value of $$\sqrt{\lambda_i}$$ put on the column $$i$$ of the $$\Sigma$$ matrix and $$x_i$$ is the eigenvector associated with the eigenvalue $$\lambda_i$$. Since we only have two eigenvalues and eigenvectors to fill for $$u_1$$ and $$u_2$$, $$u_3$$ can be calculated by computing the [null space](https://en.wikipedia.org/wiki/Kernel_(linear_algebra)) of $$A^T$$ which is $$\left[
  \begin{matrix}
  1 \\
  -2 \\
  1
  \end{matrix}
\right]$$ divided by the magnitude of the null space matrix which is $$\sqrt{1^2 + (-2)^2 + 1^2} = \sqrt{6}$$.

$$
u_1 = \frac{1}{\sqrt{3}} \left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right] \left[
  \begin{matrix}
  0 \\
  1
  \end{matrix}
\right] = \left[
  \begin{matrix}
  \frac{1}{\sqrt{3}} \\
  \frac{1}{\sqrt{3}} \\
  \frac{1}{\sqrt{3}}
  \end{matrix}
\right] \\
u_2 = \frac{1}{\sqrt{2}} \left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right] \left[
  \begin{matrix}
  1 \\
  0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  \frac{1}{\sqrt{2}} \\
  0 \\
  - \frac{1}{\sqrt{2}}
  \end{matrix}
\right] \\
u_3 = \frac{1}{\sqrt{6}} \left[
  \begin{matrix}
  1 \\
  -2 \\
  1
  \end{matrix}
\right] = \left[
  \begin{matrix}
  \frac{1}{\sqrt{6}} \\
  - \frac{2}{\sqrt{6}} \\
  \frac{1}{\sqrt{6}}
  \end{matrix}
\right]
$$

So our $$U$$ matrix is as follows.

$$
U = \left[
  \begin{matrix}
  \frac{1}{\sqrt{3}} & \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{6}} \\
  \frac{1}{\sqrt{3}} & 0 & - \frac{2}{\sqrt{6}} \\
  \frac{1}{\sqrt{3}} & - \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{6}} \\
  \end{matrix}
\right]
$$

Calculating it manually is quite cumbersome, so I'm using Matlab once again to verify the result.

```matlab
U = [1/sqrt(3) 1/sqrt(2) 1/sqrt(6); 1/sqrt(3) 0 -2/sqrt(6); 1/sqrt(3) -1/sqrt(2) 1/sqrt(6)]
S = [sqrt(3) 0; 0 sqrt(2); 0 0]
V = [0 1; 1 0]

U * S * V'
```

From the calculation, I got the matrix $$\left[
  \begin{matrix}
  1 & 1 \\
  0 & 1 \\
  -1 & 1
  \end{matrix}
\right]$$ which is equals to the matrix $$A$$.

Matlab has a function to compute the singular value decomposition of a matrix and we can simply use the function as follows.

```matlab
A =  [1 1; 0 1; -1 1]

[U, S, V] = svd(A) # U * S * V' is equal to A
```

# Extra Notes

[This video](https://www.youtube.com/watch?v=NsNNI_-JPUY) provides a visualization of how a plot is transformed by the $$U$$, $$\Sigma$$, and $$V^T$$ matrices.

# References

[Singular Value Decomposition (the SVD)](https://www.youtube.com/watch?v=mBcLRGuAFUk)

[Orthogonal matrix](https://en.wikipedia.org/wiki/Orthogonal_matrix)

[Orthogonal matrices preserve angles and lengths \| Linear Algebra \| Khan Academy](https://www.youtube.com/watch?v=yDwIfYjKEeo)

[Explainer: Orthogonal Matrices](https://www.nagwa.com/en/explainers/476190725258/)

[Diagonal matrix](https://en.wikipedia.org/wiki/Diagonal_matrix)

[Identity matrix](https://en.wikipedia.org/wiki/Identity_matrix)

[Singular Value Decomposition (SVD) tutorial](https://web.mit.edu/be.400/www/SVD/Singular_Value_Decomposition.htm)

[SVD computation example](https://www.d.umn.edu/~mhampton/m4326svd_example.pdf)

[How to find Singular Value Decomposition quick and easy - Linear algebra explained right](https://www.youtube.com/watch?v=Ls2TgGFfZnU)

[Eigenvalues and eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

[GNU Octave](https://www.gnu.org/software/octave/)

[MATLAB](https://www.mathworks.com/products/matlab.html)

[Kernel (linear algebra)](https://en.wikipedia.org/wiki/Kernel_(linear_algebra))

[A geometrical interpretation of the SVD](https://www.youtube.com/watch?v=NsNNI_-JPUY)
