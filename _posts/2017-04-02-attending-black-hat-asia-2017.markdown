---
layout: post
title:  "Attending Black Hat Asia 2017"
date:   2017-04-02 00:00:00
description: Wow, Much Learning, So Awesome
tags: Security
---

As in the previous years, this year's Black Hat Asia event is held at Marina Bay Sands in Singapore. But unlike the previous years, this time I actually got the opportunity to attend the event and see the briefings.

There are lots of interesting topics presented. The briefings are held in parallel sessions, so I had to choose one to attend for each scheduled session. I'm looking forward for the time when they're available on Black Hat's official YouTube channel.

### Briefings of Interest

I attended all the briefings I could attend, but here are some which I found really interesting. I found them relatable to the problems I encounter at work.

#### _Why We are Not Bulding a Defendable Internet_ by Halvar Flake (Keynote)

Halvar addresses several approaches organizations and the market take regarding security which are far from ideal.

How the current security products serve more for reducing risk on the CISO instead of actually securing the organization as mentioned in the talk, leading to proliferation of security product categories that only ends up for the CISO to have at more security products to buy.

He also mentioned the broken incentive system in companies, where people who add more code is more often rewarded compared to people who remove unnecessary code. According to Halvar, 90% of software risks are in pieces of code that gives less than 10% benefit.

Each line of code running on our server poses risk to the organization, and so is each software installed in the system. Even if the piece of code or the software serves as a security measure, such as sandboxing, it's still able to introduce risks to the organization.

Secure software can be written, only if the developers consider the risk when writing the code.

#### _Domo Arigato, Mr. Roboto: Security Robots a la Unit-Testing_ by Seth Law

Enforcing security testing in automated unit test and integration test is the main topic in this talk. According to Seth, security testing in software development life cycle mostly falls under quality assurance process.

QA testers and engineers should have knowledge regarding common vulnerabilities in the platform they're testing for, and add test cases to ensure no such vulnerability presents in the system. They don't really need to go as far as exploitation though, they only need to know whether the flaw exists or not.

For the software developers, security awareness regarding the platform they're developing for is crucial for them to write secure software. Seth has a simple web application that's vulnerable on purpose to be used to raise awareness for developers in his company. By showing them where the application is vulnerable and how the vulnerable code looks like, he teaches the developers how to prevent the vulnerability before it happens.

I've been planning to add security tests in the integration tests for the company I'm currently working for, so this talk is also interesting in another way. I'm glad to see someone else already did it and looks like it works for him.

#### _The Seven Axioms of Security_ by Saumil Shah (Keynote)

Vulnerabilities will stay around forever, and nothing can be truly secure. Especially because the method of defense as deployed by many organizations are not designed to stop an attack before it happens.

The defensive methods deployed in many organizations are reactive. They add new attack signatures to prevent attacks, only after the attacks happened. The security products using static rules, signatures, and machine learning couldn't stop an attack before it can be classified as an attack by human and added into the repository of knowledge referred by the security products.

Besides, the existing defensive measures aren't designed to keep up with attacker tactics. Attackers don't follow standards and definitely don't stick to methodologies taught on certification trainings. Yet, the defenders do stick to the said standards and methodologies. It's as if the security team's purpose is passing audits instead of defending from attackers.

Security and compliance aren't the same thing, as the goal of security is to defend from attackers and the goal of compliance is to comply with the rule so the company's allowed to run their business. Security team and compliance team might need to be split, so each can focus on their own jobs.

### Other Interesting Stuff

Of course those three aren't the only interesting ones. There are a lot of other interesting stuff, such as Shadow-Box kernel protector, how Michael Schwartz and Manuel Weber found a vulnerability in communication channel between virtual machines, the Go DoS vulnerabilities that's caused by the language design as presented by Roberto Clapis, and many more.

I really love the talks where I was presented with technical stuff I haven't got my hand into. Sometimes it's not so easy to understand, since I haven't got enough knowledge on that domain. Hopefully I can get my hands deeper on the machines and my head deeper on the concepts, so I can improve my skills and understanding regarding the overall system and the security concerns regarding it.

Black Hat Asia 2017 was a blast, and I'm really amazed as it's my first time attending Black Hat's event. If there's another opportunity for the next year's event, I think I'm definitely going for it.