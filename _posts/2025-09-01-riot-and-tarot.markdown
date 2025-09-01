---
layout: post
title: "Riot and Tarot"
date: 2025-09-01 00:00:00
description: Probabilistic explanation of Twitter tarot readings during Indonesia's riots
tags: Random Statistics
---

While monitoring the situation in the midst of [a series of riots](https://www.aljazeera.com/gallery/2025/8/31/indonesia-protesters-clash-with-riot-police-as-tensions-soar) happening in Indonesia starting in late August 2025, I encountered several Twitter accounts doing tarot readings on [Prabowo Subianto](https://en.wikipedia.org/wiki/Prabowo_Subianto), the President of the Republic of Indonesia. It was started by one account, but then some other accounts started doing the same thing.

Then I noticed that in the few readings I saw, all of them have [The Tower](https://en.wikipedia.org/wiki/The_Tower_(tarot_card)) card in them. Given that The Tower is a [Major Arcana](https://en.wikipedia.org/wiki/Major_Arcana) usually associated with the downfall of someone or something, which was generally the tarot readers' interpretation of what was going to happen.

I didn't explore that many tarot reading attempts people did on Prabowo. But I saw one tweet saying that, since in all of the tarot readers' readings they saw The Tower always appeared, by consensus, they believed that the things and events we expect to be represented by The Tower are going to happen.

# Probability of Pulling The Tower on Tarot Readings

A tarot deck consists of 22 Major Arcana cards and 56 [Minor Arcana](https://en.wikipedia.org/wiki/Minor_Arcana) cards. In total, we have 78 cards in a tarot deck.

When doing a reading, tarot readers usually draw a number of cards from the shuffled deck depending on how they'd like to read it. I believe there are rules on how to decide the number of cards to be drawn and how to spread it on the table when doing the reading, but I don't know how to do tarot reading (and I don't quite understand the tarot reading tutorials I found that well) so I'll just go with 6 cards since that was what the number I saw the majority of the tarot readers on Twitter drew.

The formula for calculating the probability of drawing a desired card from a deck of cards without replacement can be calculated using the [hypergeometric distribution](/2020-05-16/binomial-distribution-and-hypergeometric-distribution) formula.

$$
P(X = k) = \frac{
\left(
  \begin{matrix}
    K \\
    k \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    N - K \\
    n - k \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    N \\
    n \\
  \end{matrix}
\right)
}
$$

With that formula, we can define that from the total number of cards in a tarot deck $$N = 78$$, we're going to draw a number of cards $$n = 6$$. From the whole deck, there's only $$K$$ card which is The Tower where $$K = 1$$, and we want to calculate the probability of us getting $$k$$ The Tower card in the first $$n = 6$$ draws where $$k = 1$$ .

So when the probability of us having The Tower card on a draw of 6 cards from a deck of 78 tarot cards is as follows.

$$
P(X = 1) = \frac{
\left(
  \begin{matrix}
    1 \\
    1 \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    78 - 1 \\
    6 - 1 \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    78 \\
    6 \\
  \end{matrix}
\right)
}
$$

I'm calculating it using the [hypergeometric module probability mass function](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.hypergeom.html) on `scipy` to make my life easier, and got the following result.

```py
>>> import scipy
>>> scipy.stats.hypergeom.pmf(1, 78, 1, 6)
np.float64(0.07692307692307693)
```

That means when 100 tarot readings are done with the 6-card draw, we can expect about 7 of them to have The Tower in the reading.

Considering that one reading I saw drew 8 cards instead of 6, if we assume everyone's doing the 8-card draw, the probability would be as follows.

$$
P(X = 1) = \frac{
\left(
  \begin{matrix}
    1 \\
    1 \\
  \end{matrix}
\right)
\left(
  \begin{matrix}
    78 - 1 \\
    8 - 1 \\
  \end{matrix}
\right)
}{
\left(
  \begin{matrix}
    78 \\
    8 \\
  \end{matrix}
\right)
}
$$

As before, I'm using `scipy` to calculate the probability.

```py
>>> import scipy
>>> scipy.stats.hypergeom.pmf(1, 78, 1, 8)
np.float64(0.10256410256410257)
```

So we can expect about 10 out of 100 tarot reading attempts would include The Tower in their readings.

This calculation is a bit difficult to do manually, so if we need to quickly calculate the probability of drawing The Tower card we can simply use the following formula for approximation.

$$
P = \frac{n}{N}
$$

Where $$n$$ is the number of cards we want to draw out of the deck, and $$N$$ is the total number of cards in the deck. That should give us the probability of $$P = \frac{6}{78}$$ for 6-card draws and $$P = \frac{8}{78}$$ for 8-card draws.

# How The Tower Dominated the Readings

It's likely to happen because of [sampling bias](https://en.wikipedia.org/wiki/Sampling_bias). Given the public sentiment towards the Indonesian government and President Prabowo Subianto during the series of nationwide protests and riots, it's likely that if a reader draws a set of cards with more positive interpretations, they might be reluctant to share it with others.

The one who started it all and did the first reading actually tweeted that they're going to do a tarot reading on the President some time before doing the reading. Since the original poster drew 6 cards, assuming that it was their first reading attempt on the President and they didn't redo the reading, that gave them a 7.7% chance to get The Tower in their reading on their first try and then post on Twitter.

From a probabilistic point of view, the 7.7% probability of a tarot reader pulling The Tower is relatively low. But given enough tarot reading attempts performed, let's say 200 times, we can expect to have The Tower appear in about 15 of them. 

The initial reading by the original poster might also have influenced the behavior of people who did the reading a bit later but got very different readings, discouraging them to tweet their card draws as a response to the original poster's thread.

# Conclusion

This post is meant to explain the probability of The Tower card appearing in tarot readings, assuming random shuffling and drawing, and how the results we're seeing with a lot of people pulling The Tower on their readings might not be that surprising given what we've shown from our calculations.

I completely understand that the tarot readers and the people who read their tarot readings were disappointed with the current situation with the Indonesian government, and some of them might have a lot of hope that the readings are really predicting the future as they hoped it to be.

# References

[Indonesia protesters clash with riot police as tensions soar](https://www.aljazeera.com/gallery/2025/8/31/indonesia-protesters-clash-with-riot-police-as-tensions-soar)

[Prabowo Subianto](https://en.wikipedia.org/wiki/Prabowo_Subianto)

[The Tower (tarot card)](https://en.wikipedia.org/wiki/The_Tower_(tarot_card))

[Major Arcana](https://en.wikipedia.org/wiki/Major_Arcana)

[Minor Arcana](https://en.wikipedia.org/wiki/Minor_Arcana)

[Binomial Distribution and Hypergeometric Distribution](/2020-05-16/binomial-distribution-and-hypergeometric-distribution)

[scipy.stats.hypergeom](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.hypergeom.html)

[Sampling bias](https://en.wikipedia.org/wiki/Sampling_bias)
