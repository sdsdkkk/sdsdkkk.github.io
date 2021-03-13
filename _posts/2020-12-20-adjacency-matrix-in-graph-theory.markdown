---
layout: post
title: "Adjacency Matrix in Graph Theory"
date: 2020-12-20 00:00:00
description: Representation of neighboring vertices using a matrix
tags: LinearAlgebra GraphTheory
---

There are several ways to represent the connections between vertices in a graph, one of those is the [adjacency matrix](https://en.wikipedia.org/wiki/Adjacency_matrix). An adjacency matrix is a square matrix $$N \times N$$ where $$N$$ is the number of vertices in the graph. Each row and column $$i$$ in the matrix represents the connection the vertex $$V_i$$ with the other vertices.

# Connection Representation with Adjacency Matrix

Suppose we have the [undirected graph](https://algs4.cs.princeton.edu/41graph/) $$G$$ as follows.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,1) circle (0.2in) node {A};
    \draw (5.5,2) circle (0.2in) node {B};;
    \draw (5.5,0) circle (0.2in) node {C};

    \draw [-] (2,1) -- (5,2);
    \draw [-] (2,1) -- (5,0);
  \end{tikzpicture}
</script>

We can represent the first order connections between the vertices $$A$$, $$B$$, and $$C$$ using the following matrix.

$$
M = \left[
  \begin{matrix}
  0 & 1 & 1 \\
  1 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right]
$$

The first column represents which vertices the vertex $$A$$ is connected to, the second column represents the same thing but for vertex $$B$$, and the third column is for vertex $$C$$. The rows do the same, and are mirroring the columns' values since the graph $$G$$ is undirected so the connections work both ways for the connected vertices.

Suppose we modify the graph $$G$$ into the [directed graph](https://mathworld.wolfram.com/DirectedGraph.html) as follows, where the only available directed connections are $$A \to B$$ and $$C \to A$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,1) circle (0.2in) node {A};
    \draw (5.5,2) circle (0.2in) node {B};;
    \draw (5.5,0) circle (0.2in) node {C};

    \draw [->] (2,1) -- (5,2);
    \draw [<-] (2,1) -- (5,0);
  \end{tikzpicture}
</script>

Now, we can represent the connections using the matrix $$M$$ as follows.

$$
M = \left[
  \begin{matrix}
  0 & 1 & 0 \\
  0 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right]
$$

This time, the rows are used to represent the initial vertex of the paths and the columns are used to represent the target vertex of the paths. To represent the directed path $$A \to B$$ and $$C \to A$$, we put the number of paths from $$A$$ to $$B$$ on the second column of the first row and we put the number of paths from $$C$$ to $$A$$ on the first column of the third row.

# $$n$$-th Order Connections

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,1) circle (0.2in) node {A};
    \draw (5.5,2) circle (0.2in) node {B};;
    \draw (5.5,0) circle (0.2in) node {C};

    \draw [-] (2,1) -- (5,2);
    \draw [-] (2,1) -- (5,0);
  \end{tikzpicture}
</script>

Suppose we have the adjacency matrix $$M$$ as follows for the undirected graph $$G$$ as drawn above.

$$
M = \left[
  \begin{matrix}
  0 & 1 & 1 \\
  1 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right]
$$

From the matrix $$M$$, we can calculate the second order connections between the vertices by squaring the matrix $$M$$.

$$
M^2 = \left[
  \begin{matrix}
  0 & 1 & 1 \\
  1 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right] \left[
  \begin{matrix}
  0 & 1 & 1 \\
  1 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  2 & 0 & 0 \\
  0 & 1 & 1 \\
  0 & 1 & 1
  \end{matrix}
\right]
$$

Raising the matrix $$M$$ to the power of $$n$$ will return the number of $$n$$-th order connections between one vertex in the matrix with the others, as mentioned in the matrix powers section in this [Wikipedia article](https://en.wikipedia.org/wiki/Adjacency_matrix).

The number of first order connections between vertex $$A$$ and itself is 0 because there's no path from the vertex $$A$$ directly to itself, but the number of second order connections between vertex $$A$$ and itself is 2 because there's the path $$A \to B \to A$$ and $$A \to C \to A$$.

The number of first order connections between vertex $$B$$ and $$C$$ is 0 because there's no path from the vertex $$B$$ directly to $$C$$, but the number of second order connections between vertex $$B$$ and $$C$$ is 1 because there's the path $$B \to A \to C$$. But there's no second order connection between $$B$$ and $$A$$, because the traversed paths can't end in $$A$$.

The same rule also applies for undirected graphs.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (1.5,1) circle (0.2in) node {A};
    \draw (5.5,2) circle (0.2in) node {B};;
    \draw (5.5,0) circle (0.2in) node {C};

    \draw [->] (2,1) -- (5,2);
    \draw [<-] (2,1) -- (5,0);
  \end{tikzpicture}
</script>

The following matrix represents the paths between vertices in the above graph.

$$
M = \left[
  \begin{matrix}
  0 & 1 & 0 \\
  0 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right]
$$

By raising $$M$$ to the power of $$n$$ where $$n = 2$$, we can also get number of the second order connections of the graph.

$$
M^2 = \left[
  \begin{matrix}
  0 & 1 & 0 \\
  0 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right] \left[
  \begin{matrix}
  0 & 1 & 0 \\
  0 & 0 & 0 \\
  1 & 0 & 0
  \end{matrix}
\right] = \left[
  \begin{matrix}
  0 & 0 & 0 \\
  0 & 0 & 0 \\
  0 & 1 & 0
  \end{matrix}
\right]
$$

The only vertex with the second order connection is the vertex $$C$$, connected to $$B$$ through the path $$C \to A \to B$$.

# Extra Notes

As mentioned in [this article](https://www.baeldung.com/cs/graphs), adjacency matrix requires more memory if implemented in a program due to its requirement to store the graph information in the form of an $$N \times N$$ matrix. This might not be very efficient especially in the case where we have a lot of vertices and only a few of those vertices are connected to each other, which translates to a very sparse adjacency matrix.

In this case, we might want to use [adjacency list](https://en.wikipedia.org/wiki/Adjacency_list) instead. But it's not as straightforward to check if two vertices are connected on the $$n$$-th order using adjacency list and how many connections they have, since we need to build a tree and traverse the tree to check the connections instead of simply performing a matrix multiplication.

# References

[Adjacency matrix](https://en.wikipedia.org/wiki/Adjacency_matrix)

[Undirected Graphs](https://algs4.cs.princeton.edu/41graph/)

[Directed Graph](https://mathworld.wolfram.com/DirectedGraph.html)

[Graph Data Structures](https://www.baeldung.com/cs/graphs)

[Adjacency list](https://en.wikipedia.org/wiki/Adjacency_list)
