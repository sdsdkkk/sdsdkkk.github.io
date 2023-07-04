---
layout: post
title: "Bayesian Networks for Reliability Analysis"
date: 2023-07-04 00:00:00
description: Modeling reliability on distributed systems
tags: SoftwareEngineering Networks Statistics
---

I stumbled upon [this paper](http://arxiv.org/abs/2306.13334) by Bibartiu et al. which is a collaboration between the University of Stuttgart and Robert Bosch GmbH. After reading it, I decided to write this note on [Bayesian network](https://en.wikipedia.org/wiki/Bayesian_network) and how it can be applied for reliability analysis on distributed systems.

# Bayesian Network

Bayesian network is a representation of events that correlate to each other in the form of a [directed acyclic graph (DAG)](https://en.wikipedia.org/wiki/Directed_acyclic_graph). Each events can be assigned a probability score of it happening, where whether the event happens or not will affect the probability of one or more other events in the network getting triggered.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (0,2) circle (0.2in) node {A};
    \draw (2,2) circle (0.2in) node {B};
    \draw (1,0) circle (0.2in) node {C};

    \draw [->] (0.2,1.5) -- (0.75,0.5);
    \draw [->] (1.8,1.5) -- (1.25,0.5);
  \end{tikzpicture}
</script>

Consider nodes $$A$$, $$B$$, and $$C$$ as shown in the graph above, where the value of incident on node $$n \in \{A, B, C\}$$ is either $$T$$ when the incident happens or $$F$$ when the incident does not happen. Suppose incident on node $$C$$ will happen if both node $$A$$ and $$B$$ are having an incident.

$$
C = A \land B
$$

Therefore, we can calculate the probability of an incident happening on node $$C$$ as follows.

$$
P(C) = P(A | B)
$$

Then, suppose the incident dependency graph among $$A$$, $$B$$, and $$C$$ is as follows.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (0,2) circle (0.2in) node {A};
    \draw (2,2) circle (0.2in) node {B};
    \draw (1,0) circle (0.2in) node {C};

    \draw [->] (0.5,2) -- (1.5,2);
    \draw [->] (1.8,1.5) -- (1.25,0.5);
  \end{tikzpicture}
</script>

Incident on node $$A$$ will cause incident on node $$B$$, which in turn will cause incident on node $$C$$. Node $$B$$ can have incidents independently of node $$A$$, which will also end up causing incidents on node $$C$$.

$$
C = A \lor B
$$

So we can formulate the probability of an incident on $$C$$ as follows.

$$
P(C) = P(A) + P(B) - P(A | B)
$$

This model can only capture the probability of the incident at one point in time, which we'll need to combine with a model that can capture the mean time to failure (MTTF) and mean time to repair (MTTR) of the service running on the nodes.

# Failure Over Time and Recovery Time Estimation

This area seems to be an appropriate application of [exponential distribution](/2020-07-03/exponential-distribution) and [Weibull distribution](/2020-07-14/weibull-distribution). But due to how specialized Weibull distribution is for survival analysis, I'd say we can use Weibull distribution to model the MTTF of each nodes and during failures we can calculate the probability of the node recovering on the next time step by using [normal distribution](https://en.wikipedia.org/wiki/Normal_distribution) to model its expected MTTR.

So, each node should have its time-step measured on how long it has been on its current state (whether it's having an outage or not) and for each time step $$t$$ we can calculate the likelihood of it failing or recovering on time step $$t + 1$$.

Using this assumption, we should be able to make an estimation of the time when a certain node will experience an outage at a certain timestamp based on the last recorded time of failure of the node and also project when the node will recover.

But probability changes on one node can cascade to many other nodes and causing them all to require recalculation, so real-time probability recalculation is probably not a preferable approach for large-scale distributed systems with a lot of dependencies between components. In this case, regular recalculation of the probability of each node having a failure might be more useful for reporting and risk analysis, along with system architecture planning to ensure high standard in terms of system availability and reliability.

The real-time probability recalculation approach is probably reasonable to be implemented in systems with very few components on it (which can include a system with abstractions of a very large systems as its dependent components), but in this case it's probably more reasonable to just model it as one whole's system probability of failure, MTTF, and MTTR.

# References

[Availability Analysis of Redundant and Replicated Cloud Services with Bayesian Networks](http://arxiv.org/abs/2306.13334)

[Bayesian network](https://en.wikipedia.org/wiki/Bayesian_network)

[Directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph)

[Exponential Distribution](/2020-07-03/exponential-distribution)

[Weibull Distribution](/2020-07-14/weibull-distribution)

[Normal distribution](https://en.wikipedia.org/wiki/Normal_distribution)
