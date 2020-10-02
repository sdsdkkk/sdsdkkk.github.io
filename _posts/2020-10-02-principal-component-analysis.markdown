---
layout: content
title: "Principal Component Analysis"
date: 2020-10-02 00:00:00
description: Notes on the mathematical concepts required for principal component analysis
---

[Principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) is the process of computing the principal components for multivariate data points. The principal components can be used to help simplify the analysis by reducing the number of dimensions involved in the computation required during the actual statistical analysis. The method was introduced by [Karl Pearson](https://en.wikipedia.org/wiki/Karl_Pearson).

The features of the original dataset is analyzed based on their [correlation](https://corporatefinanceinstitute.com/resources/knowledge/finance/correlation/) and [covariance](https://www.investopedia.com/articles/financial-theory/11/calculating-covariance.asp) with each other. From here, we can reduce the set of original features into a few principal components to be analyzed further.

The principal components are derived from the original fetures in the dataset. Two features which has high correlation or covariance with one another can be converted into one principal component, which reduces the two-dimensional visualization of the two features into only one dimension as a principal component.

# Covariance and Correlation Matrix

The formula for [covariance](https://en.wikipedia.org/wiki/Covariance) is as follows.

$$
cov(X, Y) = E[XY] - E[X] E[Y]
$$

While the formula for correlation using [Pearson's correlation coefficent](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) is as follows.

$$
\rho_{XY} = \frac{cov(X, Y)}{\sigma_X \sigma_Y}
$$

Both are measurements of relationship between two variables. Pearson's correlation coefficient ranges from -1 to 1, where the plus and minus signs show whether the variables are positively or negatively correlated and the value between 0 to 1 or -1 shows how significant the correlations are.

Suppose we want to construct a covariance matrix $$C$$ between the variables $$X$$ and $$Y$$, the matrix $$C$$ can be constructed as follows.

$$
C = \left[
    \begin{matrix}
    cov(X, X) & cov(X, Y) \\
    cov(Y, X) & cov(Y, Y) \\
    \end{matrix}
  \right]
$$

$$cov(X, Y)$$ should be equal to $$cov(Y, X)$$. The covariance matrix should be symmetrical diagonally.

For a correlation matrix, we can simply replace the covariance with correlation in the resulting matrix.

$$
C = \left[
    \begin{matrix}
    \rho_{XX} & \rho_{XY} \\
    \rho_{YX} & \rho_{YY} \\
    \end{matrix}
  \right]
$$

For principal component analysis, we're going to use covariance matrix since it describes the spread of the data in two-dimensional space with the features $$X$$ and $$Y$$ better.

# Eigenvectors and Eigenvalues

An [eigenvector](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors) is a vector $$\vec{x}$$ where the product of a matrix $$A$$ and a the vector $$\vec{x}$$ is as follows.

$$
A \vec{x} = \lambda \vec{x}
$$

Where $$\lambda$$ is a real number which can be defined as $$\lambda \in \mathbb{R}$$, and $$\vec{x} \neq 0$$.

An eigenvector is a vector in the linear space that doesn't move from its original space after the transformation, but only scaled by a certain value which is its eigenvalue. If the eigenvalue is a negative number, the eigenvector is scaled to the opposite direction but the line drawn by the eigenvector stays on its original place.

[This video](https://www.youtube.com/watch?v=glaiP222JWA) gives a brief explanation on how to calculate eigenvectors and eigenvalues and its applications. [This other video](https://www.youtube.com/watch?v=PFDu9oVAE-g) gives a more detailed look into how eigenvectors and eigenvalues work.

$$x$$ is the eigenvector of matrix $$A$$ if the product of the matrix $$A$$ and vector $$\vec{x}$$ equal to $$\lambda \vec{x}$$ where $$\lambda$$ is a scalar real number. $$\lambda$$ is the eigenvalue of the eigenvector $$\vec{x}$$.

To calculate the eigenvalue $$\lambda$$, we can perform the following steps.

$$
A \vec{x} = \lambda \vec{x} \\
A \vec{x} = \lambda I \vec{x} \\
A \vec{x} - \lambda I \vec{x} = 0 \\
(A - \lambda I) \vec{x} = 0
$$

Where $$I$$ is the identity matrix of $$A$$. The determinant of the resulting matrix of $$(A - \lambda I)$$ must be equal to 0, the $$\lambda$$ value that fits the equation can be considered the eigenvalue only if $$\lambda \in \mathbb{R}$$. Otherwise, the matrix doesn't have any eigenvalue. [This video](https://www.youtube.com/watch?v=Ip3X9LOh2dk) explains determinant with visualization to help understand it better.

An eigenvector can be considered as the axis of the linear transformation. Only a square matrix can have eigenvectors.

# Principal Components

From what we already know in the previous sections, we can use covariance matrix to represent the spread of data points in a two-dimensional plane with features $$X$$ and $$Y$$ as the x-axis and y-axis. The covariance matrix is guaranteed to be a square matrix.

We also knows that we can use eigenvectors to find the vector $$\vec{x}$$ which can be used as the axis of transformation, and for the eigenvector $$\vec{x}$$ of a square matrix $$A$$ there exists a scalar value $$\lambda \in \mathbb{R}$$ whose value scales the eigenvector if we transform the square matrix $$A$$ with the eigenvector $$\vec{x}$$.

From here, we can see that the covariance matrix fulfills the shape requirement for it to have eigenvectors and eigenvalues.

$$
\left[
  \begin{matrix}
  cov(X, X) & cov(X, Y) \\
  cov(Y, X) & cov(Y, Y) \\
  \end{matrix}
\right] \left[
  \begin{matrix}
  x_1 \\
  x_2 \\
  \end{matrix}
\right] = \lambda \left[
  \begin{matrix}
  x_1 \\
  x_2 \\
  \end{matrix}
\right]
$$

The eigenvectors found on the process can be used as principal component candidates, as an eigenvector forms a vector line in the two-dimensional plane on which the data point spreads can be projected onto. The vector can be used as a principal component, which is a derived feature which is acquired from analyzing data points between two other features and creating a new vector on which the data point spread can be mapped onto. [This article](https://towardsdatascience.com/pca-eigenvectors-and-eigenvalues-1f968bc6777a) by Valentina Alto gives a good explanation and visualization of the process.

As mentioned in the chapter 3 of [this study note](http://www.cs.otago.ac.nz/cosc453/student_tutorials/principal_components.pdf) by Lindsay Smith from the computer science department of the Univesity of Otago, when the covariance matrix has more than one eigenvectors the principal component should be the eigenvector with the highes eigenvalue. The eigenvectors show vectors on which the relationship between data dimensions can be mapped into a one-dimensional line, the eigenvector with the highest eigenvalue shows the most significant relationship between the data dimensions.

The data points in the two-dimensional plane can then be projected to the principal component eigenvector to get the principal component scores. [This article](https://alliance.seas.upenn.edu/~cis520/wiki/index.php?n=Lectures.PCA) for the University of Pennsylvania gives a visualization on the projection.

[This video](https://www.youtube.com/watch?v=HD8ErvT5tnU) by Barbara Havrot shows how [vector and scalar projections](https://en.wikipedia.org/wiki/Vector_projection) are done. We can use the algebraic scalar projection formula to calculate where each data points are projected on the principal component eigenvector.

The following is the formula to calculate the scalar projection $$a'$$ of the vector $$\vec{a}$$ on the vector $$\vec{b}$$.

$$
a' = \frac{\vec{a} \cdot \vec{b}}{\lvert \vec{b} \rvert}
$$

Where $$\lvert \vec{b} \rvert$$ is the [norm](https://en.wikipedia.org/wiki/Norm_(mathematics)) of the vector $$\vec{b}$$. Since we're assuming an Euclidean space, we can use the Euclidean norm formula to calculate the norm $$\lvert \vec{b} \rvert$$.

$$
\lvert \vec{b} \rvert = \sqrt{b_{1}^2 + b_{2}^2 + b_{3}^2 + ... + b_{n}^2}
$$

After getting our principal components and principal scores, we can use the principal components in the analysis as features to be analyzed.

# Extra Notes

Machine Learning Mastery has [this article](https://machinelearningmastery.com/principal-components-analysis-for-dimensionality-reduction-in-python/) showing how to use scikit-learn to perform dimensionality reduction using PCA.

# References

[Principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis)

[Karl Pearson](https://en.wikipedia.org/wiki/Karl_Pearson)

[Correlation](https://corporatefinanceinstitute.com/resources/knowledge/finance/correlation/)

[Calculating Covariance for Stocks](https://www.investopedia.com/articles/financial-theory/11/calculating-covariance.asp)

[Covariance](https://en.wikipedia.org/wiki/Covariance)

[Pearson correlation coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)

[Eigenvalues and eigenvectors](https://en.wikipedia.org/wiki/Eigenvalues_and_eigenvectors)

[Eigenvalues & Eigenvectors: Data Science Basics](https://www.youtube.com/watch?v=glaiP222JWA)

[Eigenvectors and eigenvalues \| Essence of linear algebra, chapter 14](https://www.youtube.com/watch?v=PFDu9oVAE-g)

[The determinant \| Essence of linear algebra, chapter 6](https://www.youtube.com/watch?v=Ip3X9LOh2dk)

[PCA: Eigenvectors and Eigenvalues](https://towardsdatascience.com/pca-eigenvectors-and-eigenvalues-1f968bc6777a)

[A tutorial on Principal Components Analysis](http://www.cs.otago.ac.nz/cosc453/student_tutorials/principal_components.pdf)

[PCA](https://alliance.seas.upenn.edu/~cis520/wiki/index.php?n=Lectures.PCA)

[Vectors 7.5 Scalar and Vector Projections](https://www.youtube.com/watch?v=HD8ErvT5tnU)

[Vector projection](https://en.wikipedia.org/wiki/Vector_projection)

[Norm (mathematics)](https://en.wikipedia.org/wiki/Norm_(mathematics))

[Principal Component Analysis for Dimensionality Reduction in Python](https://machinelearningmastery.com/principal-components-analysis-for-dimensionality-reduction-in-python/)
