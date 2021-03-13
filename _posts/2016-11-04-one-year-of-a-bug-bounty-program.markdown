---
layout: post
title:  "One Year of a Bug Bounty Program"
date:   2016-11-04 00:00:00
description: Learning by Running a Bug Bounty Program
tags: Security
---

It's been a year since [Bukalapak](https://www.bukalapak.com), the company I'm working for, paid our first bounty reward to [Roberto Urbanus](https://twitter.com/Bet0_Shinoda). Roberto found multiple vulnerabilities on our site and reported it.

We never considered running a bug bounty program before, so we seriously considered it when Roberto asked if there's any reward for the reported vulnerabilities. After discussing it internally, we decided to give Roberto bounty reward and merchandise as a token of appreciation.

Soon, we're having other bounty hunters searching for vulnerabilities in our system. Until recently, we didn't have any dedicated personnel in our company to ensure the security of our products and systems.

Our development team do have code review process, we also have a short guide on secure development. But the lack of personnel in our engineering and technology team pushed our engineers to focus on projects for company growth. So we relied on bounty hunters to find and report vulnerabilities.

In most cases of reported vulnerabilities, I'm the one responsible to communicate with the reporters and patching the vulnerabilities. It gave me a chance to talk with lots of people who reported vulnerabilities. They're from various countries with different backgrounds, and a lot of them are really good at finding vulnerabilities. Some of them make a living from bug bounty programs, some other do it just for fun.

I've been interested in computer systems security for quite some time, and I found running a bug bounty program is a great way to learn. It helps me to think more thoroughly regarding the risks contained in the code I write and how it could be abused.

I consider myself quite a paranoid, and as a software engineer I develop software with safeguards in case of abuse. But running a bug bounty program taught me that even being paranoid when writing code might not guarantee the security of the program without a deeper understanding on how each components are interacting with each other.

It's also not enough to be the only paranoid one in the development team. We need to help others to be aware of the risks regarding the code they write. Sometimes the vulnerabilities are caused by a simple line of code that's easy to miss during code reviews, or a simple authorization code that's skipped in the logic.

Recently, I started focusing on the security aspects of the system. Shifting my job from a software engineer to a security engineer in the last two months, I find it enjoyable to spend my time learning about how things work and how each piece interacts with each other to see if there's any possible scenario where it can be exploited.

Running a bug bounty program really helped. But a company shouldn't rely too much on a bug bounty program for its security. Otherwise, the company might suffer when an attacker started looking for vulnerability with the intention to exploit it.

A bug bounty program also have a well-defined terms and conditions regarding where the security researchers should look for bugs. A real attacker wouldn't comply to that when they're attacking.