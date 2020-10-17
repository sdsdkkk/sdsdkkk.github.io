---
layout: content
title: "Matrix Eigendecomposition"
date: 2020-10-17 00:00:00
description: Factorization of a square matrix, eigenvalues, and eigenvectors
---

[Eigendecomposition](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix) is the factorization of a matrix, where the matrix is represented by its [eigenvalues and eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors).

Suppose we have the matrix $$A$$ whose dimension is $$n \times n$$, $$A$$ can be factorized as follows.

$$
A = Q \Lambda Q^{-1}
$$

Where $$Q$$ is the square $$n \times n$$ matrix whose $$i$$-th column is the eigenvectors $$q_i$$ of $$A$$, and $$\lambda$$ is the diagonal matrix whose diagonal element $$\lambda_i$$ is the corresponding eigenvalue of the eigenvector $$q_i$$.

The decomposition can be derived from the fundamental property of eigenvectors, which is as follows.

$$
A v = \lambda v
$$

Where $$v$$ is an eigenvector of $$A$$ and $$\lambda$$ is the eigenvalue of $$v$$.

$$
A Q = Q \Lambda \\
A = Q \Lambda Q^{-1}
$$

Only [diagonalizable matrices](https://en.wikipedia.org/wiki/Diagonalizable_matrix) can be factorized using eigendecomposition. [This video](https://www.youtube.com/watch?v=KTKAp9Q3yWg) shows how eigendecomposition is computed.

# Computing $$\Lambda$$ and $$Q$$

To perform eigedecomposition on a matrix, we need to compute the eigenvalues and eigenvectors of the matrix $$A$$.

$$
A x = \lambda x
$$

After we get the eigenvalues, we can construct the diagonal matrix $$\Lambda$$ using the values of each individual eigenvalue $$\lambda$$ as a diagonal element of the diagonal matrix. The following is a $$\Lambda$$ matrix for a $$3 \times 3$$ matrix eigendecomposition.

$$
\Lambda = \left[
  \begin{matrix}
  \lambda_1 & 0 & 0 \\
  0 & \lambda_2 & 0 \\
  0 & 0 & \lambda_3
  \end{matrix}
\right]
$$

The matrix $$Q$$ can be constructed by using the eigenvectors $$q_i$$ of the matrix $$A$$ as the columns of the matrix $$Q$$, ordered according to which column in the $$\Lambda$$ matrix is occupied by the eigenvalue $$\lambda_i$$ associated with the eigenvector $$q_i$$.

$$
Q = \left[
  \begin{matrix}
  q_1 & q_2 & q_3
  \end{matrix}
\right]
$$

Suppose we have the matrix $$A$$ as follows.

$$
A = \left[
  \begin{matrix}
  2 & 0 & 1 \\
  0 & 1 & 0 \\
  0 & 0 & 1
  \end{matrix}
\right]
$$

The following is the Matlab code to perform the eigendecomposition process and check that the eigenvalues and eigenvectors match the equation $$A x = \lambda x$$ previously stated.

```matlab
A = [2 0 1; 0 1 0; 0 0 1]

[V, D, W] = eig(A)

# Checking if the eigenvectors and eigenvalues are correct
# if plugged into the equation A * x = lambda * x
A * V(:, 1) == D(1, 1) * V(:, 1)
A * V(:, 2) == D(2, 2) * V(:, 2)
A * V(:, 3) == D(3, 3) * V(:, 3)
```

# Computing Eigenvalues and Eigenvectors

To compute eigenvalues and eigenvectors of a matrix $$A$$, we need to perform the following steps.

$$
A x = \lambda x \\
A x = \lambda I x \\
A x - \lambda I x = 0 \\
(A - \lambda I) x = 0
$$

From here we need to calculate the determinant of $$(A - \lambda I)$$ and compute the equation as follows.

$$
\lvert (A - \lambda I) \rvert x = 0
$$

Suppose we have a $$2 \times 2$$ matrix $$A$$ as follows.

$$
A = \left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right]
$$

We can follow the steps previously described with the matrix $$A$$.

$$
A x = \lambda x \\
\left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = \lambda I \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] \\
\left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = \lambda \left[
  \begin{matrix}
  1 & 0 \\
  0 & 1
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] \\
\left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = \left[
  \begin{matrix}
  \lambda & 0 \\
  0 & \lambda
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] \\
\left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] - \left[
  \begin{matrix}
  \lambda & 0 \\
  0 & \lambda
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = 0 \\
(\left[
  \begin{matrix}
  2 & 1 \\
  1 & 2
  \end{matrix}
\right] - \left[
  \begin{matrix}
  \lambda & 0 \\
  0 & \lambda
  \end{matrix}
\right]) \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = 0 \\
\left[
  \begin{matrix}
  2 - \lambda & 1 \\
  1 & 2 - \lambda
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = 0
$$

We already reached the point of $$(A - \lambda I) x = 0$$, now we can calculate the determinant $$\lvert (A - \lambda I) \rvert$$.

$$
\left|
  \begin{matrix}
  2 - \lambda & 1 \\
  1 & 2 - \lambda
  \end{matrix}
\right| x = 0 \\
((2 - \lambda) ( 2 - \lambda) - (1) (1)) x = 0 \\
(4 - 4 \lambda + \lambda^2 - 1) x = 0 \\
(3 - 4 \lambda + \lambda^2) x = 0 \\
3 - 4 \lambda + \lambda^2 = 0 \\
\lambda^2 - 4 \lambda + 3 = 0 \\
(\lambda - 1) (\lambda - 3) = 0
$$

Therefore, we can have the values of $$\lambda$$ to be $$\lambda = 1$$ and $$\lambda = 3$$. These are the eigenvalues of the matrix $$A$$.

To calculate the eigenvectors, we can use [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) as instructed in [this document](https://www.scss.tcd.ie/Rozenn.Dahyot/CS1BA1/SolutionEigen.pdf) from the Trinity College Dublin. The Gaussian elimination is performed for the matrices as follows, with the eigenvalue $$\lambda$$ plugged into the matrix to find the respective eigenvector $$x$$ of the eigenvalue $$\lambda$$.

$$
(A - \lambda I \lvert 0)
$$

We're aiming to transform the matrices using Gaussian elimination until we have the value of $$x_1$$ and $$x_2$$ of the eigenvector $$x = \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right]$$ for the eigenvalue $$\lambda$$.

For $$\lambda = 1$$, we can perform Gaussian elimination with the following matrix.

$$
\left[
  \begin{matrix}
  2 - \lambda & 1 & \lvert & 0 \\
  1 & 2 - \lambda & \lvert & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  2 - 1 & 1 & \lvert & 0 \\
  1 & 2 - 1 & \lvert & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  1 & 1 & \lvert & 0 \\
  1 & 1 & \lvert & 0
  \end{matrix}
\right]
$$

For $$\lambda = 3$$, the matrix is as follows.

$$
\left[
  \begin{matrix}
  2 - \lambda & 1 & \lvert & 0 \\
  1 & 2 - \lambda & \lvert & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  2 - 3 & 1 & \lvert & 0 \\
  1 & 2 - 3 & \lvert & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  -1 & 1 & \lvert & 0 \\
  1 & -1 & \lvert & 0
  \end{matrix}
\right]
$$

But in this case, we don't need to perform the Gaussian elimination since the resulting equations are already clear. For $$\lambda = 1$$, we get the values of $$x_1$$ and $$x_2$$ as follows.

$$
x_1 + x_2 = 0 \\
x_1 = - x_2
$$

So we can compute the eigenvector as follows.

$$
x = \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = \left[
  \begin{matrix}
  x_1 \\
  - x_1
  \end{matrix}
\right] = x_1 \left[
  \begin{matrix}
  1 \\
  -1
  \end{matrix}
\right]
$$

We get the eigenvector $$x = \left[
  \begin{matrix}
  1 \\
  -1
  \end{matrix}
\right]$$ for $$\lambda = 1$$.

While for $$\lambda = 3$$ we get this.

$$
- x_1 + x_2 = 0 \\
x_1 - x_2 = 0 \\
x_1 = x_2
$$

So we can get the eigenvector as follows.

$$
x = \left[
  \begin{matrix}
  x_1 \\
  x_2
  \end{matrix}
\right] = \left[
  \begin{matrix}
  x_1 \\
  x_1
  \end{matrix}
\right] = x_1 \left[
  \begin{matrix}
  1 \\
  1
  \end{matrix}
\right]
$$

We get the eigenvector $$x = \left[
  \begin{matrix}
  1 \\
  1
  \end{matrix}
\right]$$ for $$\lambda = 3$$.

With the eigenvectors and eigenvalues we have, we can factorize $$A$$ as follows.

$$
A = Q \Lambda Q^{-1}
$$

Where $$Q$$, $$\Lambda$$, and $$Q^{-1}$$ are defined below.

$$
Q = \left[
  \begin{matrix}
  1 & 1 \\
  -1 & 1
  \end{matrix}
\right] \\
\Lambda = \left[
  \begin{matrix}
  1 & 0 \\
  0 & 3
  \end{matrix}
\right] \\
Q^{-1} = \left[
  \begin{matrix}
  \frac{1}{2} & - \frac{1}{2} \\
  \frac{1}{2} & \frac{1}{2}
  \end{matrix}
\right]
$$

We can check the result using Matlab.

```matlab
A = [2 1; 1 2]
Q = [1 1; -1 1]
L = [1 0; 0 3]

A == Q * L * Q^-1
```

# Extra Notes

[This video](https://www.youtube.com/watch?v=i8FukKfMKCI) by Zach Star shows how eigenvectors can be used to determine the equilibrium of a system and some other stuff.

# References

[Eigendecomposition of a matrix](https://en.wikipedia.org/wiki/Eigendecomposition_of_a_matrix)

[Eigenvalues and eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

[Diagonalizable matrix](https://en.wikipedia.org/wiki/Diagonalizable_matrix)

[Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination)

[Finding Eigenvalues and Eigenvectors](https://www.scss.tcd.ie/Rozenn.Dahyot/CS1BA1/SolutionEigen.pdf)

[The applications of eigenvectors and eigenvalues \| That thing you heard in Endgame has other uses](https://www.youtube.com/watch?v=i8FukKfMKCI)
