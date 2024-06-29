---
layout: post
title: "On the AI Hype"
date: 2024-06-29 00:00:00
description: It probably won't live up to its hype
tags: SoftwareEngineering ComputerScience Random
---

Not too long ago, I stumbled upon [this blog post](https://ludic.mataroa.blog/blog/i-will-fucking-piledrive-you-if-you-mention-ai-again/) and found it pretty amusing. So I shared it with my friends and coworkers, and soon enough I immediately remembered that just a few months prior I had a chat with a friend in India about the AI hype among Indian startups.

So the friend told me that in India, every startup founder he knew was rushing to incorporate AI into their startups and some investors made statements along the lines of, "If your company isn't using AI, then it's not worth investing in." Because of this, he felt like he might be missing out and asked me if I thought AI would be a game changer as it has been claimed to be.

While I can't claim myself to be an expert in AI, I did read quite a lot of papers in AI throughout my one paper per day for the whole year reading streak since January 2021. So I should be quite well-informed about how the current state-of-the-art AI models work and their weaknesses.

In short, I answered that I believe that LLMs (which are what people will think of if we say "AI" nowadays) have many legit use cases, unlike blockchain which was the flagship of the previous hype train. But the state-of-the-art LLMs have a lot of weaknesses that need to be addressed properly if we want to implement a solution based on it, and to fix some of the weaknesses is to make a breakthrough in the field of AI itself.

# Context Window and Hallucination

LLMs have limited [context windows](https://winder.ai/llm-prompt-best-practices-large-context-windows/) that often lead it to mix things up between the information we've already provided it with once the amount of information we fed to the LLM goes past a certain threshold. This makes it very risky to rely on it to make autonomous decisions given a certain set of information we want it to use as context.

And even without the context issue, we already have to deal with [hallucinations](https://circleci.com/blog/llm-hallucinations-ci/) issues built into the LLM which may cause the LLM to make up things and give us the wrong information. This makes LLMs highly unreliable for making decisions and generating content, and we must verify everything that the LLMs return in order to avoid bigger problems it might cause later.

The fact that LLMs are built to be probabilistic instead of deterministic may also cause us issues since it may contain various inconsistencies in its responses where we need it to follow a certain set of rules properly. This is where we're better off writing traditional software code than using LLMs.

# Interfacing with Various Systems

While there are works such as [this](https://arxiv.org/abs/2402.06664) and [this](https://arxiv.org/abs/2404.08144) from Fang et al. which demonstrated LLMs' abilities to perform tasks autonomously (in this case, hacking a system), in reality, it's quite difficult to integrate LLMs into a system to act as the brain that controls various components autonomously.

As mentioned in the previous section, LLMs are built to be probabilistic. When we put an LLM to interface with various systems, each with a different API and CLI command convention structure, the AI might not be able to reliably generate a response that follows the correct format to instruct the target system to perform an action.

And if we want the LLM to act as an autonomous brain of the system, it needs to be able to interpret every single output from the systems it's interfacing with correctly. Sometimes the interaction context can be quite long if the LLM must perform a relatively complex task that involves analyzing a series of outputs from the target system to determine what instruction the LLM should send next. Remember the problem with context windows and hallucinations we discussed in the previous section? That's going to be a problem here also.

# Measuring Model Performance on Production Environments

For traditional machine learning models, it might be relatively easy for us to detect degradations in its performance after being deployed to production by monitoring how off the model's output is from the expected result given the data point processed by the model. This is because it's relatively easy to quantify the model's performance on a given task, and if there's [any issue](https://dcai.csail.mit.edu/2024/imbalance-outliers-shift/) that needs to be addressed by retraining the model to better fit the data they're going to deal with in production.

It might not be as straightforward to monitor and evaluate LLMs' performance, except in cases where we restrict it to a very limited set of actions. But once we restrict the LLM to only be able to perform very simple tasks, we probably can solve it in a cheaper and more reliable way by writing the code directly or by using traditional machine learning solutions.

# Conclusion

As far as I see, LLMs are pretty useful and have a lot of potential in their applications. But given that LLMs' main selling point is in their ability to emulate human responses and help human users summarize or generate information, I think LLMs' main strength is in improving the UX of systems that might not be very intuitive to human users to use due to the volume or interpretability of the information that needs to be digested by the human user.

For interfacing directly with various computer systems and acting as an autonomous system, I think LLMs are still unreliable and it's better to avoid using them for such things especially if the system is mission critical.

# References

[I Will Fucking Piledrive You If You Mention AI Again](https://ludic.mataroa.blog/blog/i-will-fucking-piledrive-you-if-you-mention-ai-again/)

[LLM Prompt Best Practices For Large Context Windows](https://winder.ai/llm-prompt-best-practices-large-context-windows/)

[LLM hallucinations: How to detect and prevent them with CI](https://circleci.com/blog/llm-hallucinations-ci/)

[LLM Agents can Autonomously Hack Websites](https://arxiv.org/abs/2402.06664)

[LLM Agents can Autonomously Exploit One-day Vulnerabilities](https://arxiv.org/abs/2404.08144)

[Class Imbalance, Outliers, and Distribution Shift](https://dcai.csail.mit.edu/2024/imbalance-outliers-shift/)
