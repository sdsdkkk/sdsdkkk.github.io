---
layout: post
title: "Infrastructure Design and Operations Decoded"
date: 2025-03-07 00:00:00
description: A knockoff of the book Software Design Decoded, in the form of a blog post
tags: SoftwareEngineering Networks Security
---

This post contains some notes from my discussions with a few colleagues and friends over the past few months regarding how to design a system. I'm going to write it in a style similar to the book [Software Design Decoded: 66 Ways Experts Think](https://www.goodreads.com/book/show/29889476-software-design-decoded), but without the picture illustrations.

Consider this post a knockoff of the book, but in the form of a blog post and talking more about infrastructure design and operations.

## Compartmentalize Your Networks

Avoid making one big network environment that's accessible to everyone when you have a relatively big organization. Make your networks small enough and compartmentalize your system components to each network according to the business context served by the systems.

## Assume Deployment to Different Isolated Environments

As a horizontal team, when designing a system to be used by a large organization with compartmentalized business units, don't assume your system components will always communicate with each other through an internal network. Design it for communication through the Internet.

## Minimize Scaling Dependencies Between Two or More compartmentalized Systems

Design your system so that if one business vertical needs to scale, only the components within the scope of that business vertical's cluster need to be scaled. If it depends on a cluster owned by another business vertical (or a shared cluster managed by a horizontal team), it will add more communication overhead and more people to be involved during the scaling coordination which will slow down your scaling process.

## Your Zero Trust Might Be Different from My Zero Trust

One person/company's interpretation of how a proper zero trust architecture implementation looks like might be different from another's. Just focus on what principles that need to be addressed and focus on pushing the architecture towards complying with those principles.

## How Identical Do You Want It To Be?

Ideally, a staging environment is identical to the production environment it's supposed to mimic. The problem is how identical it should be, considering that a staging environment should be managed differently regarding access management requirements, public access restrictions, and infrastructure cost considerations. There are multiple layers in the system starting from its low-level hardware and networking layers up to its application layers, set expectations with your dev teams on which layers you're going to guarantee identical setup.

## Can You Give Me Access to Perform This Action?

Some types of access can safely be granted to your devs (some of them probably require a new interface that restricts what they can do with the granted access), but some other types are better done by your infrastructure team.

## Just Because It's Cool Doesn't Mean That It's Valuable

Don't push the adoption of a certain system architecture or a certain solution just because it's the cool, trendy thing. Make sure it's actually solving your problem and is valuable to the company. If your leadership personnel does that, try to educate them not to.

## Just Because Big Tech X Does It Doesn't Mean You Should

"Microsoft did this" and "Facebook did this" aren't good reasons for you to do something. Microsoft and Facebook might be even doing the exact opposite things yet it works for them because of their different needs. Know your company's needs well and think from there.

## Why Learn X That Deep When You Can Just Mimic What the Others Do?

"Why do you waste so much time relearning the fundamentals of distributed systems, just read some blog posts from Netflix and do what they do." You won't be able to execute as well as Netflix if you don't understand the underlying principles and fundamentals of what they do when mimicking it.

## Understand How People Think and Behave

Your infrastructure systems and platforms are going to determine how other people in the company think about their systems in their interaction with yours. Make sure to set it up in a way that encourages them to behave in your intended manner and in the most productive way possible.

# References

[Software Design Decoded: 66 Ways Experts Think](https://www.goodreads.com/book/show/29889476-software-design-decoded)
