---
layout: post
title: "Basic Concepts of Word Embedding"
date: 2020-12-25 00:00:00
description: Representing words as vectors for natural language processing
tags: MachineLearning ComputerScience
---

[Word embedding](https://machinelearningmastery.com/what-are-word-embeddings/) is a method to represent words as vectors, where the words with similar meaning are identified with vectors similar to each other. [This article](https://towardsdatascience.com/word-embeddings-exploration-explanation-and-exploitation-with-code-in-python-5dac99d5d795) gives a more thorough explanation on the general working of word embedding.

As per December 2020 [word2vec](https://arxiv.org/pdf/1301.3781v3.pdf) is the standard model used for word embedding, which utilizes CBOW and skip-gram in its implementation and is explained further in [this paper]((https://arxiv.org/pdf/1411.2738.pdf)). We're going to take a look at CBOW and skip-gram, along with a few other relevant concepts for word embedding.

# Bag of Words

[Bag of words](https://en.wikipedia.org/wiki/Bag-of-words_model) model is built from a list of tokenized words from a corpus, from which we can build a matrix to store the frequency of appearance of the words.

Suppose we have the following text from a tweet $$X$$ as our text data.

> Manchester United lost the game last Saturday against Wolverhampton Wanderers, while Manchester City won the game against Liverpool.

Using the bag of words model $$BoW_1$$, we can build the following table representation.

| Word          | Occurences |
|---------------|------------|
| Manchester    | 2          |
| United        | 1          |
| lost          | 1          |
| the           | 2          |
| game          | 2          |
| last          | 1          |
| Saturday      | 1          |
| against       | 2          |
| Wolverhampton | 1          |
| Wanderers     | 1          |
| while         | 1          |
| City          | 1          |
| won           | 1          |
| Liverpool     | 1          |

Let's say we scraped for another tweet $$Y$$, which content is as follows.

> Reynhard Sinaga, an Indonesian PhD student at the University of Manchester, was sentenced for his crimes.

From this tweet, we can build the following bag of words model $$BoW_2$$.

| Word          | Occurences |
|---------------|------------|
| Reynhard      | 1          |
| Sinaga        | 1          |
| an            | 1          |
| Indonesian    | 1          |
| PhD           | 1          |
| student       | 1          |
| at            | 1          |
| the           | 1          |
| University    | 1          |
| of            | 1          |
| Manchester    | 1          |
| was           | 1          |
| sentenced     | 1          |
| for           | 1          |
| his           | 1          |
| crimes        | 1          |

We can merge the models $$BoW_1$$ and $$BoW_2$$ using a [disjoint union](https://en.wikipedia.org/wiki/Disjoint_union) operator.

$$
BoW_3 = BoW_1 \uplus BoW_2
$$

From the union we now have the model $$BoW_3$$ as follows.

| Word          | Occurences |
|---------------|------------|
| Manchester    | 3          |
| United        | 1          |
| lost          | 1          |
| the           | 3          |
| game          | 2          |
| last          | 1          |
| Saturday      | 1          |
| against       | 2          |
| Wolverhampton | 1          |
| Wanderers     | 1          |
| while         | 1          |
| City          | 1          |
| won           | 1          |
| Liverpool     | 1          |
| Reynhard      | 1          |
| Sinaga        | 1          |
| an            | 1          |
| Indonesian    | 1          |
| PhD           | 1          |
| student       | 1          |
| at            | 1          |
| University    | 1          |
| of            | 1          |
| was           | 1          |
| sentenced     | 1          |
| for           | 1          |
| his           | 1          |
| crimes        | 1          |

Using $$BoW_3$$ as the bag of words model reference, we can represent the tweets $$X$$ and $$Y$$ as the following vectors.

$$
X = \left[
  \begin{matrix}
  2 & 1 & 1 & 2 & 2 & 1 & 1 & 2 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0
  \end{matrix}
\right] \\
Y = \left[
  \begin{matrix}
  1 & 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1
  \end{matrix}
\right]
$$

The more words we have in the bag of words model, the sparser the vector representation for each tweets will be due to the tweets not containing many of the words listed in the bag of words.

The vector representations of the corpus can then be processed using some other methods, such as using neural networks for [text classification](https://monkeylearn.com/text-classification/) or [sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis).

For the example tweets $$X$$ and $$Y$$, suppose that we want to classify the tweets taken from a news publishing Twitter account where both tweets are scraped from. We might want to classify the tweet $$X$$ as a sports news text and the tweet $$Y$$ as a criminal news text.

# TF-IDF

[TF-IDF](https://monkeylearn.com/blog/what-is-tf-idf/) is a statistical measure that evaluates how relevant a word is to a document in a collection of documents. TF stands for terms frequency, which measures how often a word appear in a document. IDF stands for inverse document frequency which measures how common a word is across documents and inverting the result, so the closer IDF to 0 the more common the word is across documents.

The TF-IDF is the product of TF of a word to a document being analyzed and IDF of the word to the collection of documents being analyzed.

$$
tfidf(t, d, D) = tf(t, d) \cdot idf(t, D)
$$

There are a few methods to calculate the weight of TF and IDF, which can be seen on [this article](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) on Wikipedia. TF-IDF can be used instead of the number of occurences of the word in a bag of words model to eliminate irrelevant words from being noises in the vector to be processed, an example of this can be seen on [this article](https://www.analyticsvidhya.com/blog/2020/02/quick-introduction-bag-of-words-bow-tf-idf/).

# Bag of N-Grams

Bag of words can be used to keep track of the individual words in the document, but it might miss the relations between words that's bound together to form a single object in the sentence. For example, the following text we used earlier.

> Manchester United lost the game last Saturday against Wolverhampton Wanderers, while Manchester City won the game against Liverpool.

Manchester United, Wolverhampton Wanderers, and Manchester City are two words strung together to name one object which is an English football club.

We can use [bag of n-grams](https://machinelearning.wtf/terms/bag-of-n-grams/) with $$n = 2$$ for the sentence as follows.

| 2-grams                     | Occurences |
|-----------------------------|------------|
| Manchester United           | 1          |
| United lost                 | 1          |
| lost the                    | 1          |
| the game                    | 2          |
| game last                   | 1          |
| last Saturday               | 1          |
| Saturday against            | 1          |
| against Wolverhampton       | 1          |
| Wolverhampton Wanderers     | 1          |
| Wanderers while             | 1          |
| while Manchester            | 1          |
| Manchester City             | 1          |
| City won                    | 1          |
| won the                     | 1          |
| game against                | 1          |
| against Liverpool           | 1          |

The bag of 2-grams model retain some local information regarding how the words are used together, which is lost in the bag of words model. From this information, we can do things like identify the words commonly used together as a phrase in the documents.

# Continuous Bag of Words

[Continuous bag of words](https://www.kdnuggets.com/2018/04/implementing-deep-learning-methods-feature-engineering-text-data-cbow.html) (CBOW) model is used to find a target word given the words surrounding the target word in a given context. The CBOW model is one of the architecture implemented in word2vec.

Suppose we have the sentence as follows.

> There is a heavy rainfall and thunderstorm covering the area surrounding the crime scene at the moment.

When training the CBOW model, we feed a vector representing the words before and after the target word as the input to the model and expecting the target word as the output. The following is the illustration of the model, where we're inputting two words before the target word and two words after the target word in the sentence to represent the target word.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (0,6.5) node {word(t-2)};
    \draw (0,4.5) node {word(t-1)};
    \draw (0,2.5) node {word(t+1)};
    \draw (0,0.5) node {word(t+2)};

    \draw (1,6) -- (2,6) -- (2,7) -- (1,7) -- (1,6);
    \draw (1,4) -- (2,4) -- (2,5) -- (1,5) -- (1,4);
    \draw (1,2) -- (2,2) -- (2,3) -- (1,3) -- (1,2);
    \draw (1,0) -- (2,0) -- (2,1) -- (1,1) -- (1,0);

    \draw [->] (2,6.5) -- (5,3.5);
    \draw [->] (2,4.5) -- (5,3.5);
    \draw [->] (2,2.5) -- (5,3.5);
    \draw [->] (2,0.5) -- (5,3.5);

    \draw (5.5,4.25) node {SUM};
    \draw (5,3) -- (6,3) -- (6,4) -- (5,4) -- (5,3);

    \draw [->] (6,3.5) -- (9,3.5);

    \draw (9,3) -- (10,3) -- (10,4) -- (9,4) -- (9,3);
    \draw (10.5,3.5) node {w(t)};
  \end{tikzpicture}
</script>

Suppose the sentence isn't processed in any way and we feed it directly to the CBOW model, for the target word $$w(t)$$ "rainfall" we're going to input four words in the model and train the model to relate those words to the target word "rainfall" to train the network.

```
input = ['a', 'heavy', 'and', 'thunderstorm']
output = ['rainfall']
```

The more a set of words is used in a same sequence with the target word $$w(t)$$ in the documents used for training, the more relevant the set of words is to the target word $$w(t)$$.

# Skip-Gram

[Skip-gram](https://www.kdnuggets.com/2018/04/implementing-deep-learning-methods-feature-engineering-text-data-skip-gram.html) model is used to find the related words that can be used together in a context with the target word. While CBOW is used to associate a series of words to a target word where the input is the series of words, skip-gram works in reverse. Skip-gram takes the target word as the input, and it returns the series of words closely associated to the target word.

The following architecture shows an example where the skip-gram model returns four word vectors related to the target word $$w(t)$$.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (0.5,3.5) node {w(t)};
    \draw (1,3) -- (2,3) -- (2,4) -- (1,4) -- (1,3);
    
    \draw [->] (2,3.5) -- (5,3.5);

    \draw (5,3) -- (6,3) -- (6,4) -- (5,4) -- (5,3);

    \draw [->] (6,3.5) -- (9,6.5);
    \draw [->] (6,3.5) -- (9,4.5);
    \draw [->] (6,3.5) -- (9,2.5);
    \draw [->] (6,3.5) -- (9,0.5);

    \draw (9,6) -- (10,6) -- (10,7) -- (9,7) -- (9,6);
    \draw (9,4) -- (10,4) -- (10,5) -- (9,5) -- (9,4);
    \draw (9,2) -- (10,2) -- (10,3) -- (9,3) -- (9,2);
    \draw (9,0) -- (10,0) -- (10,1) -- (9,1) -- (9,0);

    \draw (11,6.5) node {word(t-2)};
    \draw (11,4.5) node {word(t-1)};
    \draw (11,2.5) node {word(t+1)};
    \draw (11,0.5) node {word(t+2)};
  \end{tikzpicture}
</script>

Suppose we have the sentence as follows, which we used in the previous section about CBOW model.

> There is a heavy rainfall and thunderstorm covering the area surrounding the crime scene at the moment.

If the sentence isn't processed in any way and we feed it directly to the skip-gram model, for the target word $$w(t)$$ "rainfall" we're going to input the vector for the word "rainfall" into the model to train it to return the vectors for the surrounding words "a", "heavy", "and", and "thunderstorm".

```
input = ['rainfall']
output = ['a', 'heavy', 'and', 'thunderstorm']
```

The more a set of words is used in a same sequence with the target word $$w(t)$$ in the documents used for training, the heavier the association of the set of words to the target word $$w(t)$$.

# References

[What Are Word Embeddings for Text?](https://machinelearningmastery.com/what-are-word-embeddings/)

[Word embeddings: exploration, explanation, and exploitation (with code in Python)](https://towardsdatascience.com/word-embeddings-exploration-explanation-and-exploitation-with-code-in-python-5dac99d5d795)

[Efficient Estimation of Word Representations in Vector Space](https://arxiv.org/pdf/1301.3781v3.pdf)

[word2vec Parameter Learning Explained](https://arxiv.org/pdf/1411.2738.pdf)

[Bag-of-words model](https://en.wikipedia.org/wiki/Bag-of-words_model)

[Disjoint union](https://en.wikipedia.org/wiki/Disjoint_union)

[Text Classification](https://monkeylearn.com/text-classification/)

[Sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis)

[What is TF-IDF?](https://monkeylearn.com/blog/what-is-tf-idf/)

[tf-idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)

[Quick Introduction to Bag-of-Words (BoW) and TF-IDF for Creating Features from Text](https://www.analyticsvidhya.com/blog/2020/02/quick-introduction-bag-of-words-bow-tf-idf/)

[Bag-of-n-grams](https://machinelearning.wtf/terms/bag-of-n-grams/)

[Implementing Deep Learning Methods and Feature Engineering for Text Data: The Continuous Bag of Words (CBOW)](https://www.kdnuggets.com/2018/04/implementing-deep-learning-methods-feature-engineering-text-data-cbow.html)

[Implementing Deep Learning Methods and Feature Engineering for Text Data: The Skip-gram Model](https://www.kdnuggets.com/2018/04/implementing-deep-learning-methods-feature-engineering-text-data-skip-gram.html)
