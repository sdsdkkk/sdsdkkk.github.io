---
layout: content
title: "Graph Theory"
date: 2020-04-26 00:00:00
description: Exploring graph theory
---

The [seven bridges of Konigsberg](https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg).

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

# TikZJax

This article is made possible by [TikZJax](http://tikzjax.com/), a web client-side TikZ-like vector graphics tools to draw diagrams. I was looking for tools to enable me draw graphs on my articles without having to commit and upload image files to the site, I found TikZJax and went with it.

It took a bit of work to make TikZJax work, as we need to copy the BaKoMa font sets, configure path to the WASM file, etc though.

# References

[Seven Bridges of Konigsberg](https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg)

[TikZJax](http://tikzjax.com/)
