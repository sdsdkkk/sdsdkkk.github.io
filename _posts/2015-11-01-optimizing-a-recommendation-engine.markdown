---
title:  "Optimizing a Recommendation Engine"
date:   2015-11-01 00:00:00
description: Might Be a Little Too Optimistic
---

This post is based on a personal experience and thoughts. It might not be academically valid, but just take it as a simple thought on the topic.

Well, I haven't really proven it to be effective. It's not like I'm an expert in the field too. But hey, it sounds like a good idea to me. Besides, I think we already have academic works proposing ideas similar to this (see related works).

### Recommendation System

Let's say I'm a regular user of an online music player app (let's call it MuGeek). MuGeek can track the songs I listen to, and the developers want to give me recommendations based on the songs I've listened before.

From here, there are two common approaches they can use.

#### User-Based

I'm a Japanese music fan. I listen to Takahashi Yuu a lot using MuGeek. If MuGeek has accumulated enough users, it might be almost certain that I'm not the only Japanese music fan among their users, and there's a huge chance that I'm not the only one who listen to Takahashi Yuu regularly.

By using user-based recommendation, MuGeek will measure how much I like each music I listened to. Then, MuGeek will compare my preferences with other users'. After deciding which users have the most similar music preferences with me, they find out that these other users regularly listen to artists that I haven't listened to.

One of the artists those other users listen to a lot is Galneryus. Deciding that Galneryus might be a good recommendation for me, MuGeek will then recommend Galneryus' songs to me.

#### Item-Based

This time, MuGeek tries to recommend a few songs to me when I'm listening to another song. I'm currently listening to a song titled "(Where's) The Silent Majority?" and they want to give me a song recommendation right at that time.

To give me the recommendation, they check the other songs they have and see which songs people usually listen to together with the song I'm currently listening to.

After they find that people who listen to "(Where's) The Silent Majority?" are also likely to listen to "Never Let This Go" and several other songs, MuGeek will recommend "Never Let This Go" and the other songs to me when I listen to "(Where's) The Silent Majority?"

### The Engine

While both of the methods mentioned before are different in how they decide what to recommend, both of them will produce a table like this.

~~~
Entity   | ItemA      ItemB      ItemC     ItemD     ItemE
----------------------------------------------------------
    E1   |  0.0        0.7        0.8       1.0       0.1
    E2   |  0.8        0.8        0.0       1.0       0.0
~~~

In terms of user-based recommendation, the entity would be users, and the item ratings show how much is the likelihood that each user will like each item. For MuGeek, the items would be the songs in MuGeek's database.

In terms of item-based recommendations, looking at the table above, we can recommend ItemD, ItemC, and ItemB to users viewing item E1. For users viewing item E2, we can recommend ItemD, ItemA, and ItemB.

In the case of MuGeek and their songs, the ratings would show how likely a user listening song E1 and E2 will listen to the other songs.

The problem with this engine is that to recommend an item based on an entity, the system needs to process the entity's data and compare it to every other record in the system. We can save the data to database and query it later to fetch recommendations for users.

~~~
Entity  | Target  | Score
-------------------------
    E1  | ItemA   |  0.0
    E1  | ItemB   |  0.7
    E1  | ItemC   |  0.8
    E1  | ItemD   |  1.0
    E1  | ItemE   |  0.1
    E2  | ItemA   |  0.8
    E2  | ItemB   |  0.8
    E2  | ItemC   |  0.0
    E2  | ItemD   |  1.0
    E2  | ItemE   |  0.0
~~~

When the system has so much data, saving the ratings into a database table to be queried later will take a lot of time.

But it's also really time-consuming when the system has a huge amount of data to be processed, so it's not really feasible to calculate the similarity on the fly for entities with no previous calculated ratings.

### Clustering and Naive Bayes

Here's the big idea. By clustering the entities and targets, we should be able to reduce the number of the stored data in our database.

We can imagine from the previous section that the recorded entities and targets in the database are the representations of each individual users and items in our system.

If MuGeek have 3 million users and 10 million songs, they'll have 30 million rating information for user-based scores and 100 million for item-based scores.

By using unsupervised clustering algorithms, MuGeek can let their system cluster the users and songs. Then, they can keep only the relations between the generated clusters of users and clusters of songs in their scoring table. This way, MuGeek will save more storage space as they don't need to keep records for individual users and songs.

Take the table below as an illustration.

~~~
EntityCluster  | TargetCluster  | Score
---------------------------------------
    ECluster1  | ItemClusterA   |  0.5
    ECluster1  | ItemClusterB   |  0.2
    ECluster1  | ItemClusterC   |  0.4
    ECluster1  | ItemClusterD   |  0.2
    ECluster1  | ItemClusterE   |  0.0
    ECluster2  | ItemClusterA   |  0.9
    ECluster2  | ItemClusterB   |  0.8
    ECluster2  | ItemClusterC   |  0.4
    ECluster2  | ItemClusterD   |  1.0
    ECluster2  | ItemClusterE   |  0.3
~~~

So now, how will we fetch the recommendation for individual users and songs? Good question, and that's why we'll have a Naive Bayes classifier.

After we cluster each individual entities, we should be able to know which cluster they should belong to according to our clustering algorithm. So we can have the entity - cluster pair of data. These entity - cluster pairs can be used to train Naive Bayes classifiers.

When we request for recommendation based on an individual entity, the Naive Bayes classifier will see which cluster the entity would fit the most. Then, we can fetch the recommendation for the returned cluster from database.

We can also get recommendations for entities we didn't have on the training results by using the Naive Bayes classifier, as we can classify them into one of our clusters and fetch the recommendations belong to the cluster.

In short, the recommendation serving steps turned from this:

~~~
request for entity X
  |> fetch recommendation record for entity X
       |> serve recommendation
~~~

To this:

~~~
request for entity X
  |> classify entity X to a cluster
       |> fetch recommendation for the cluster
            |> serve recommendation
~~~

### Problems

The recommendation from the engine implementing this might not be very personalized, as it recommends for a group of entities instead of only one entity. But as long as the recommendation is relevant to the users' interest, this shouldn't be a big deal.

The other problem would be balancing the recommendation results. So far, I haven't thought about it to the extent where the system can give a balanced recommendation result from the recommended cluster of items.

We can randomize the items from the recommended cluster or we can recommend top items from the recommended cluster. The possibility of users within the same cluster have different preferences is pretty high.

Maybe we should break down the clusters into sub-clusters and calculate the ratings between sub-clusters too.

### Related Works

[Clustering Technique for Collaborative Filtering Recommendation and Application to Venue Recommendation](http://www.slideshare.net/PhamCuong/clustering-technique-for-collaborative-filtering-recommendation-and-application-to-venue-recommendation)

[Online Learning for Latent Dirichlet Allocation](https://www.cs.princeton.edu/~blei/papers/HoffmanBleiBach2010b.pdf)
