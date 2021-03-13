---
layout: post
title: "Konigsberg Bridge Problem"
date: 2020-05-01 00:00:00
description: Revisiting Konigsberg bridge problem and the Euler's circuit
tags: GraphTheory
---

This article is originally a test page for a few JavaScript libraries and plugins I can use to write code to be processed as rendered graph on the client side. Since I was testing it using the Konigsberg bridge problem as the test graph, I decided to simply write a note about it to not waste the effort I've spent on drawing the graph representation of the problem. Also, it's a good chance to revisit the graph theory for a bit.

I first learned about the Konigsberg bridge problem when I was on my first semester in computer science major back in 2009. I didn't really come to appreciate it until a few years later though.

# Seven Bridges of Konigsberg

The [seven bridges of Konigsberg](https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg) or the Konigsberg bridge problem was a historical problem in the field of mathematics. [Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler) proved that there is no way to start from one point at the city and going back to the same point by going through all of the bridges exactly once, and the proof is considered as the first theorem in graph theory.

The following graph describes the Konigsberg bridge problem as graph with vertices $$A$$, $$B$$, $$C$$, and $$D$$ connected by edges.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (8,6) circle (0.2in) node {A};
    \draw (4,6) circle (0.2in) node {B};
    \draw (12,6) circle (0.2in) node {C};
    \draw (8,0) circle (0.2in) node {D};
    \draw (4.5,6.1) to [bend left=10] (7.5,6.1);
    \draw (8.5,6.1) to [bend left=10] (11.5,6.1);
    \draw (4.5,5.9) to [bend right=10] (7.5,5.9);
    \draw (8.5,5.9) to [bend right=10] (11.5,5.9);
    \draw (8,5.5) to (8,0.5);
    \draw (4.3,5.6) to (7.7,0.4);
    \draw (11.7,5.6) to (8.3,0.4);
  \end{tikzpicture}
</script>

We can define the above graph as $$G = (V, E)$$ where vertices $$V = \{A, B, C, D\}$$ and edges $$E = \{AB, BA, AC, CA, AD, BD, CD\}$$.

The degree of a vertex $$v$$ is defined as the number of paths represented by edges connected to the vertex. The Konigsberg bridge problem is a multigraph where there can be multiple edges connecting the same nodes, so all edges will be counted. For $$v \in V$$, the degree of the vertex $$deg(v)$$ is as follows.

$$
deg(A) = 5 \\
deg(B) = 3 \\
deg(C) = 3 \\
deg(D) = 3
$$

Notice that every vertex in the graph $$G$$ has an odd degree. For every time we're traversing a degree out of any vertex $$v$$ in the graph to complete the cycle to traverse every edge once and come back to the starting vertex, we're leaving an even number of remaining edges to be traversed from the vertex $$v$$. Eventually, there will be a time where we come back to the vertex $$v$$ with only one remaining edge to be traversed.

Therefore, it's impossible to traverse all of the edges exactly once and come back to the starting vertex because there will be no case where we can traverse back to the starting vertex without having to leave the vertex again due to the one last remaining edge to be traversed from the vertex. To be able to finish the cycle at the starting vertex, the starting vertex need to have an even degree to ensure that for every edge we traverse to leave the vertex there's another edge we can use to come back to the vertex.

Let's take a look at the following vertex $$D$$ with degree $$deg(D) = 3$$ from the graph $$G$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (8,0) circle (0.2in) node {D};
    \draw (8,2.5) to (8,0.5);
    \draw (6,2.5) to (7.7,0.4);
    \draw (10,2.5) to (8.3,0.4);
  \end{tikzpicture}
</script>

Suppose we choose $$D$$ as the starting point, when we choose to leave the vertex using edge $$e \in \{AD, BD, CD\}$$ we have two remaining untraversed edges. Let's say we manage to traverse all of the other edges exactly once and come back to $$D$$ by traversing one of the two remaining edges, we're going to have one remaining untraversed edges that we need to traverse to fulfill the requirement to traverse every edges in graph $$G$$ exactly once.

Thus, we can't both traverse all of the existing edges exactly once and also return to the starting vertex. If we stay at the vertex $$D$$ upon returning, we don't traverse all of the edges since there's at least one edge connected to the vertex $$D$$ that we haven't traversed. Yet, if we traverse that edge we have no way of returning to the vertex $$D$$ while fulfilling the requirements since there's no remaining untraversed edge connected to $$D$$ for us to use in the return trip.

Suppose we choose any other vertex as the starting point and we have the vertex $$D$$ with the degree $$deg(D) = 3$$. To traverse all edges and return to the starting vertex to complete the cycle, we need to enter the vertex $$D$$ using one of the edges $$e$$ where $$e \in \{AD, BD, CD\}$$. Upon entering the vertex $$D$$ we traversed one of the edges and upon leaving $$D$$ we traversed another one of the edges. We can't come back to $$D$$ using the only remaining edge and return to the starting vertex without traversing at least one of the three edges connected to $$D$$ twice. If we insist on traversing the edges only once at maximum, when we come back to $$D$$ we have no way of leaving $$D$$. Also if we insist on traversing all of the edges without using any edge twice as the requirement states, we have no choice but to come back to $$D$$ using the one remaining edge connected to $$D$$ after entering and leaving $$D$$ which leaves us to be stuck at $$D$$ without being able to return to the starting vertex.

So we can't have any of the vertices having an odd degree for us to be able to traverse through all of the edges exactly once and being able to return to the starting vertex, and the graph $$G$$ in the Konigsberg bridge problem doesn't fulfill the requirements that for graph $$G = (V, E)$$ every edge can be traversed exactly once and back to the starting vertex only if for every vertex $$v \in V$$ the degree $$deg(v)$$ is even.

Graphs that allows for every edge to be traversed exactly once and back to the starting vertex of the traversal are later called [Eulerian graphs](http://mathonline.wikidot.com/eulerian-graphs-and-semi-eulerian-graphs) after Euler. Also, the traversal cycle is called Eulerian cycle or Eulerian circuit.

# Real-Life Application

Eulerian circuit theorem is used to determine whether a defined set of paths can be traversed exactly once for every path that needs to be traversed in the map. The obvious real-life application is to choose travel routes, for example a mail delivery courier who needs to stop by a lot of places along the way in order to deliver items.

I never really knew the application of Eulerian circuit aside from that kind of cases, and seems like I still couldn't find anything outside of picking a route to ensure all of the roads are traveled. From what I see so far it seems that the concept of Eulerian circuit is more well known due to the influence it has in laying the foundation of graph theory, which is an important field of mathematics for the modern world.

Graph theory has a lot of real-life applications in the field of computer science and software engineering, and choosing the most efficient route from one vertex to another is an important aspect in implementing dynamic routing algorithms in computer networks.

# Post-World War II Konigsberg

During World War II, the city of Konigsberg was bombarded by the Soviets. Two of the seven bridges were destroyed during the bombing.

The city is now called [Kaliningrad](https://en.wikipedia.org/wiki/Kaliningrad) in the honor of the Soviet leader [Mikhail Kalinin](https://en.wikipedia.org/wiki/Mikhail_Kalinin). The city was renamed in 1945, it was taken to be a part of the Soviet Union at the end of the war.

# TikZJax

This article is made possible by [TikZJax](http://tikzjax.com/), a web client-side TikZ-like vector graphics tools to draw diagrams. I was looking for tools to enable me draw graphs on my articles without having to commit and upload image files to the site, I found TikZJax and went with it.

It took a bit of work to make TikZJax work, as we need to copy the BaKoMa font sets, configure path to the WASM file, and some other adjustments though.

# References

[Seven Bridges of Konigsberg](https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg)

[Leonhard Euler](https://en.wikipedia.org/wiki/Leonhard_Euler)

[Eulerian Graphs and Semi-Eulerian Graphs](http://mathonline.wikidot.com/eulerian-graphs-and-semi-eulerian-graphs)

[Kaliningrad](https://en.wikipedia.org/wiki/Kaliningrad)

[Mikhail Kalinin](https://en.wikipedia.org/wiki/Mikhail_Kalinin)

[TikZJax](http://tikzjax.com/)
