---
layout: post
title:  "What Do Hackers Do?"
date:   2018-11-04 00:00:00
description: A Brief Introduction
tags: Security
---

### Background

One of my friends who's not working in tech nor information security wanted to know about hackers, how attacking and defending are conducted in the cyber space, and how to catch a cyber criminal. She's trying to write a fictional story on this topic, and didn't feel like the resources she got from the news help that much.

I explained it to her and some others, then one of them suggested me to write it down in a blog post instead. So here it is.

Her main questions were:

- What do hackers do, actually?
- How do you catch one?

To answer this, we're going to talk a bit about how the "hacker" terminology started, and also how defensive countermeasures and forensics investigation works in general.

### What Are Hackers, Anyway?

To answer the question on what hackers actually do, we need to see what the term "hacker" actually means. While hackers are usually associated with cyber attacks and cyber criminals nowadays, the term actually means something different.

[Help Net Security has an article on the history of hacking](https://www.helpnetsecurity.com/2002/04/08/the-history-of-hacking/), which I told her to read since it was on the first page when I googled for a reference to give her about the history of hacking before I started explaining things. The article gives us a quick glimpse of how the terminology's popular meaning changed from "some kind of geek hobbyist" to "cyber criminals".

In my own words, I'd say hackers are a bunch of system enthusiasts who explore an existing system beyond what regular people do. Through their explorations, they gain deeper knowledge regarding the system which may lead them to knowing some tricks to get around the system's limitations that regular users are usually bound to.

Getting around the system's limitations sometimes involves bypassing the system's security mechanisms applied by the developers to avoid reverse engineering and protecting their intellectual property. This is a part where hackers intersect with the field of security. Some systems are designed with minimal protection to allow tinkerers study the system internals, so playing with open systems doesn't require the hacker to break any rules applied by the developers.

Since regular users don't know the system as well as the hackers, the hackers' tricks might seems like magic when shown to them. But it's all just curiosity, added with proper analysis of the system from what information is available to the hackers.

The tricks a hacker knows might allow him to access parts of the system that's not available to regular users without the understanding of the nitty-gritty details of how the system works. When brought to a multi-user software systems which is interconnected through computer networks, the parts accessible by the hacker might be some sensitive information or configuration that might harm some other people if abused. This is another part where hackers intersect with the field of security.

A hacker might build a software or a simple script to automate his exploits on the system. These automated exploits can then be distributed to other parties, which may or may not understand how the system actually works while executing the automated exploits they got from the hacker. Some exploits might not be easy to automate, so the hacker can simply write a step-by-step guide to perform the exploit and distribute the tutorial to other people. The users of these exploits might be malicious and planning to use the exploits for their personal gains, which turned these exploit users into cyber attackers. Therefore, cyber attackers aren't necessarily hackers themselves since they might not have the knowledge and hands-on experience regarding their target system's internals.

### How Do You Know What Happened in the System?

To answer the question on how to catch a cyber criminal, we first need to understand how developers, system administrators, and forensic investigators can deduce what happened inside the system and identify who the perpetator whenever an incident happens.

A good developer must have some kind of mechanisms for him to detect problems in the system he built. For software, it's usually built in the form of logs. The more detailed logs the developers keep, the better visibility we have over what's happening in the system according to the logs written by the software.

A system administrator generally manages a system infrastructure consisted of network devices, system software, and application software. Assuming the components of the system managed by the system administrator are built by competent developers, the system administrator should be able to configure the logging functions of the system components and access the logs whenever they should investigate anything in the system.

A forensic investigator can extract data from the computer's parts and try to reconstruct the incident. A computer consisted of electrical and mechanical parts, which in general is categorized into CPU, memory, storage, and I/O devices. In the lowest level of computer systems, we have these parts complying to the law of physics. A forensic investigator might take the parts of a computer where an incident took place into custody for analysis.

I won't dive into the details of software development, system administration, and digital forensics methodologies here to explain the best practices for us to be able to respond quickly to incidents whenever there's an attack. Given you have some understanding on computer architecture, computer networks, data structures, and operating systems, you should be able to look for some computer security and digital forensics tutorials to get a better understanding on how things work technically.

When an attacker infiltrate our system, something that's out of the norm might be recorded in the system logs. An example is when one of our users' account is compromised and hijacked by an attacker, the user who usually logs in from city A using an IP address block from ISP X might suddenly logs in from city C using IP address that belongs to ISP Z. While it might only be a case of the user having a vacation in city C, the incident is worth noting so we can get back to it whenever we detected malicous activities by the user's account.

Actually identifying the attacker is a bit more complicated and highly dependent on how careful the attacker is when performing the attack. But [Krebs on Security](https://krebsonsecurity.com/) should have coverage on notable security incidents and investigations that might be useful for references.

### Conclusion

This post is meant to give a quick introduction to hacker culture and information security for people from outside of the tech field. Specifically, it's meant to summarize the answer I gave to my friend who needs a bit of a guidance so she can write fictional stories about hackers and information security somewhat realistically.

I suggest watching [Mr Robot](https://www.imdb.com/title/tt4158110/) or [Blackhat](https://www.imdb.com/title/tt2717822/) to see a bit on realistic hacking techniques in fictional stories. Okay, Blackhat's reputation isn't very good. But I don't watch that many movies and TV shows so those two might be my best bet to introduce the techniques to people without actually teaching them the technical details. Mr Robot is a pretty good TV show though, and the techniques are also pretty realistic.