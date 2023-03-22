---
layout: post
title: "Studying Geckos"
date: 2023-03-22 00:00:00
description: Gecko observation report
tags: Zoology DataScience
---

When the pandemic started in 2020, I started spending most of my time at home in late March as we're forced to go into WFH. After a while, I noticed that I encountered geckos quite often in the house. Sometimes I could see them, but many times I just heard their voices and cleaned up their droppings.

In June 2020, I came up with an idea to test the use of various sounds on geckos to see if it scares them or not. Some examples of the sounds I used are sound from a tone generator app, music from YouTube videos, and recorded sounds of the geckos' natural predators such as cats, hawks, and owls.

So I started keeping track of gecko sightings inside the house, the sound I used on the geckos (if any), and the gecko's reactions to it, along with some other information I could think of that might or might not be relevant. The following is a glimpse of the collected data.

![Sample data from the dataset](/images/posts/gecko-observation-dataset.png)

By the time I exported it for analysis on March 2023, the tracker had 78 sightings recorded.

My plan didn't go well though, since my initial hypothesis of the geckos having meaningful reactions to the sounds I used on them quickly proven to be false. Initially, I noticed some geckos reacted to the sound and ran into hiding when the sound was played. But after about six months doing that, I found that the number of times when the geckos reacted to the sound was insignificant. So I decided to drop the research on the geckos' behaviors when sounds were used. I still continue to record the gecko sightings though, and while I was at it I decided to study more about reptiles in general (which ends up with me being quite knowledgeable about lizards and snakes by 2023).

In 2022, I noticed that there are two species of geckos around the house: [_Hemidactylus frenatus_](https://en.wikipedia.org/wiki/Common_house_gecko) (the common house gecko) and [_Lepidodactylus lugubris_](https://en.wikipedia.org/wiki/Lepidodactylus_lugubris) (the mourning gecko).

This realization invalidates my data collection method, since when I started I assumed all of the geckos in my house were _Hemidactylus frenatus_. I must have encountered some _Lepidodactylus lugubris_ earlier but mistook it as juvenile _Hemidactylus frenatus_ due to its color being darker and its size being smaller.

Only after stumbling upon [this video](https://www.youtube.com/watch?v=L4CHEge5wlc) by Clint's Reptiles on YouTube I learned about mourning geckos. The geckos look pretty familiar to me as I noticed some of the geckos I encountered so far that resides around certain areas in the house have smaller head-to-body size ratio, darker color, and more timid. So I checked their range and found that mourning geckos are quite common around my area.

I also learned that _Hemidactylus frenatus_ and _Lepidodactylus lugubris_ can live close by but won't occupy the exact same territory, as _Hemidactylus frenatus_ tend to be very territorial and aggressive towards other gecko species. So I found another supporting evidence that I have two gecko species in the house: _Hemidactylus frenatus_ around the areas deeper into the hosue and _Lepidodactylus lugubris_ around the areas bordering the yard.

I took note of it, but I didn't update my data collection strategy yet to take into account the fact that I have been observing two different gecko species just yet because I haven't got that many data points in my observation records and I might still need to learn more about them in order to be able to come up with a better research plan.

While the data I currently have isn't very useful because of various mistakes I made when starting the observation, in this post I'm going to present what I manage to get from it anyway.

# Data Visualization and Analysis

The following is the chart of the number of sightings for each month up to February 2023. The data was exported on 21 March 2023, but there was no sighting throughout the month at this point.

![Monthly sightings](/images/posts/gecko-monthly-sighting-occurences.png)

I didn't put any tracker on the geckos like a proper biologists usually do when observing wild animal populations. So I estimate the size of the gecko population in the house based on the number of sources from where I heard their voices, where their droppings are, and how many I can see at the same time.

One shortcoming of the data collection method I used is that it doesn't record most of those because it was initially meant just for taking notes of each individual gecko's reactions to the sounds I used on them. Therefore, the only meaningful data I collected in the dataset that can be used to estimate the size of the gecko population around the house is how many can be seen at the same time.

![Monthly max number of geckos per sighting](/images/posts/gecko-monthly-max-number-per-sighting.png)

I can probably use the data on the gecko's color and patterns, but due to varying lighting conditions when I observed them I might've mistaken the same gecko by their dark or light colorings when checking them visually. Also, after a few months of recording the sightings I learned that [_Hemidactylus frenatus_ can change colors](https://ejournal.undip.ac.id/index.php/bioma/article/view/36617) from darker to lighter gradients and vice versa for camouflage and thermal regulation, so the color that I recorded might not be relevant for identifying them.

I recorded the aggressiveness of the geckos in the behavior column, where I categorized them into aggressive, neutral, and timid. Aggressive is when the gecko reacts to my presence by approaching me as if it's prepared to defend itself when I approach it. Neutral is when the gecko ignores my presence. Timid is when the gecko tries to stay away from me when I approach it.

![Aggression](/images/posts/gecko-aggression.png)

I only encountered one instance of an aggressive gecko, and that one I identified as _Hemidactylus frenatus_. This is consistent with information I get from various sources that _Hemidactylus frenatus_ is [a territorial and aggressive gecko species](https://animaldiversity.org/accounts/Hemidactylus_frenatus/). Most of them are neutral though, and some are quite timid when I encountered it. But for those I identified as _Lepidodactylus lugubris_, I found all of them timid.

I recorded the gecko's growth phase in the size column. I only put full-grown and juvenile as the values there, but in hindsight I should've labeled the hatchling and the very young ones as baby instead. I encountered a vew hatchling and very young ones, but I labeled them as juveniles. Also, I'm sure there were a few instances where I mistook adult _Lepidodactylus lugubris_ as juvenile _Hemidactylus frenatus_ and mislabeled them.

![Growth phase](/images/posts/gecko-growth.png)

The only column that I definitely didn't mislabel anything is the floor column. This shows whether the geckos are encountered on the first floor or on the second floor of my house, since my house is a two-story one.

![Sighting location](/images/posts/gecko-sighting-location.png)

But after giving it some thought, I think I should've split my house into several regions in order to be able to better identify the geckos' territories. On the first floor, I noticed that there are at least four regions that seem to be occupied by different geckos, three of them are occupied by _Hemidactylus frenatus_ and one is occupied by _Lepidodactylus lugubris_. On the second floor, there are five regions that also seem to be occupied by different geckos, two of them are occupied by _Hemidactylus frenatus_, two are occupied by _Lepidodactylus lugubris_, and one was occupied by _Hemidactylus frenatus_ but now seems to be occupied by _Lepidodactylus lugubris_ (the _Hemidactylus frenatus_ seem to have moved somewhere else further into the house).

# Evaluation and Future Improvements

As I previously mentioned, there are so many weaknesses in the way I conducted this research that led to relatively few insights I can derive from it. I lacked understanding about reptiles in general, and especially the geckos I'm observing, when I started.

Given that in order for a research project to produce value it requires a consistent data collection and processing methodology over a large amount of data that's collected over a long period of time, this project is very sloppily directed and I practically spent about three years to acquire the dataset which isn't very useful. The real gain for me in this project is my growing interest towards herpetology and the experience of observing animal populations (I'm not sure if I can call them wild, considering they live comfortably alongside humans).

Several points that I need to improve if I'm going to continue doing this project:

- Create proper zoning for the areas in the house in order to identify the locations of the geckos' territories.
- Incorporate gecko dropping locations and gecko voice source locations into the collected dataset.
- Stop treating all the geckos sighted as one species in the dataset, because there are clearly more than one species coexisting together in the house.

# References

[Common house gecko](https://en.wikipedia.org/wiki/Common_house_gecko)

[Lepidodactylus lugubris](https://en.wikipedia.org/wiki/Lepidodactylus_lugubris)

[Mourning Gecko, The Best Pet Lizard?](https://www.youtube.com/watch?v=L4CHEge5wlc)

[Penilaian Kamuflase Cecak Rumah Hemidactylus frenatus Duméril & Bibron, 1836](https://ejournal.undip.ac.id/index.php/bioma/article/view/36617)

[Hemidactylus frenatus](https://animaldiversity.org/accounts/Hemidactylus_frenatus/)
