---
layout: post
title: "A Software Engineering Career Reflection"
date: 2025-07-05 00:00:00
description: After almost 12 years of professional experience
tags: SoftwareEngineering Random
---

I started 2025 with some personal reflection due to having a catch-up with an old friend back in late December 2024, which led me to write [this post about the importance of learning](/2025-01-03/nothing-you-learn-is-useless). Last month, a few random events also led me to reflect on [how I learned security and why you shouldn't do it the same way](/2025-06-09/you-shouldnt-learn-security-the-way-i-did). And after that, I've been reflecting on what I've been doing so far as a software engineer.

Considering that I started this blog back in 2015 with a post about [accidentally being a software engineer](/2015-09-05/accidental-software-engineer), which was written in a manner I now consider pretty inappropriate, I must say I started out as a pretty brash engineer with a lack of awareness on how to act appropriately in social settings. Yet it worked for me, since my workplace at the time, Bukalapak, used to have so many weird people. Heck, when my late mother told me in 2014 that she used to worry about me getting into the workforce, not because she was worried about my abilities but because she was worried about me being a weird kid, I told her confidently, "No problem, the CTO at my office is even weirder than me."

But as I joined Cermati in 2017, where people were acting more professionally (in contrast to Bukalapak's more playful vibes), I started getting corrected on my behaviors by my bosses and coworkers, which ended up with me being way more presentable in formal professional settings nowadays. [I even got to manage a team](https://medium.com/cermati-tech/on-the-transition-from-being-an-individual-contributor-to-being-a-people-manager-af91282ab149), and this is my seventh year of managing teams at Cermati Fintech Group.

Since this is my twelfth year working professionally as a software engineer and my seventh year of managing an engineering team, people might think that I've got my things figured out. Well, sorry to disappoint, but I actually still have a lot to learn and improve on.

Before we get into that, let's start with how I got into software engineering.

# High School Years

When I was in high school, as I've already written in the post about [how I learned security](/2025-06-09/you-shouldnt-learn-security-the-way-i-did), I used to tinker a lot with the family PC in my family's living room.

I mostly tinkered with the Windows XP OS settings and stuff. Eventually, I got fascinated by how intricate the system configurations are, such as the Windows Registry and stuff, and wondered how to make something like that. Also, it was a period when I was still exchanging floppy disks with my friends to exchange applications, files, and other stuff, and we got a lot of computer virus infections from the floppy disk exchanges. This led me to want to learn how to make those viruses, since being able to make one was considered peak coolness among my computer hobbyist friends.

So I decided to learn Visual Basic programming on my own, since my school's IT teacher told me that we could make viruses with that. But I didn't really get anywhere when trying to learn it all by myself, given that I lacked resources beyond cheap programming books I could find at bookstores which didn't explain the programming fundamentals and just went straight to the step-by-step guides to build stuff using Visual Basic (and at times, the resources weren't for the right Visual Basic version).

My IT teacher also told me that if I wanted to try making games, I could try learning Macromedia Flash (it wasn't that long after Macromedia was acquired by Adobe). So I also learned a bit of Flash, but I never got into learning how to code ActionScript at this point (I learned it later in university and made a few games with it).

The exact time when I started to be able to program properly was when the IT teacher decided to teach the students Visual FoxPro programming at school. I think I was among the fastest to pick it up in the class, and I started having some hobby projects using Visual FoxPro at home.

Later on, the teacher finally taught us Visual Basic. But at that point, I was already comfortable with Visual FoxPro, so I kept using it for my hobby projects at home.

# University Years

As soon as I started my university years at Universitas Multimedia Nusantara (UMN), I got an algorithm class taught in C. We were instructed to use the [Dev-C++](https://www.bloodshed.net/) IDE with the C/C++ compiler for Windows packaged with it.

Since I was already using Linux (which was quite ahead of my peers at the university), I also learned how to write C code in Linux with different libraries than what I used in class (since I couldn't use the Windows-specific ones). While I was on it, I also decided to pick up Python on my own since Python was mentioned a lot in various forums I visited. I also picked up some Ruby, since I was a fan of [Diki Andeas](https://www.goodreads.com/author/show/3481230.Diki_Andeas), the author of Chickenstrip comic strip about a Linux enthusiast and Ruby on Rails programmer in a chicken costume.

From there, I learned a lot more about web, desktop, and mobile programming at university while testing various Linux distros on VirtualBox at home. I often switched distros on my laptop at the time and learned how to deal with various types of issues with the OS while I was at it. This led me to choose network engineering and systems administration courses for my optional courses at UMN, and also led me to participate in INSECxT, the university's security community.

UMN made it mandatory for students to have industry internships by their fourth year of study. But during my fourth year of study, I chose to take a Unity game project by some students from the Visual Communication Design major. Their team was recruiting a game programmer, and I could take it for my mandatory internship project. So I got some experience doing a 3D game with Unity and JavaScript at this point. I also previously learned to create Flash games with Adobe Flash and ActionScript, and made a few for university projects. So, taking on a 3D game project wasn't really that out of character for me.

Since I took a university project as my mandatory internship, how about the industry internship experience expected from the university's graduates? No worries, since I took an industry internship a year before. It didn't count for the internship coursework, but I got some interesting experience from interning as an Implementation Consultant at DataOn, primarily involving programming with Adobe ColdFusion and working with MS SQL Server, both of which I had never touched before and after that.

On the side, I also created a tool to help my non-techie friends to bypass Internet censorship enforced by Indonesia's then-Minister of ICT, [Tifatul Sembiring](https://en.wikipedia.org/wiki/Tifatul_Sembiring). The censorship primarily targeted porn contents on the Internet back then, but I had a friend who was a master's student (not in fields related to computer science) at a different university and was pretty into porn, and he asked me to make something to allow him to get around the censorship so he could access porn.

# Bukalapak

I joined Bukalapak in August 2013 as a software engineer, primarily because they use Ruby on Rails, and I was doing Ruby programming as a hobby back in university. While I was quite familiar with Ruby, I never had any experience with Rails so I had to learn it fast during my first day there from a few books given by their senior engineers to study and a link to [RailsCasts](https://rbates.dev/railscasts-retrospective-part-1-the-fuel).

When I first joined Bukalapak, Kaskus FJB (Forum Jual Beli, basically a forum to buy and sell things) was the biggest e-commerce platform in Indonesia. Bukalapak was trying to overtake them, so I was assigned to work on features that allow us to convert Kaskus FJB users' transactions to Bukalapak's e-commerce escrow system.

There were a few interesting things I did there, such as:

- Reverse engineering a multi-courier service delivery tracking application to enable us to build better delivery tracking features. Based on what I've learned from reverse engineering the application, I wrote a new packet delivery tracker system for the company that enabled us to support automatic tracking for more courier services, which enabled us to automate our transaction processing workflow more.
- Writing a Ruby-based user-space hardware driver for a banking TOTP hardware we customized to automate our transaction system's payment verification process.
- Revamping their two-step transaction checkout process into one step, but with the flexibility to turn it back into two steps if needed (which they later did).
- Winning an internal hackathon to rewrite their original implementation of a promo coupon feature, which was written by a senior engineer when I first joined, but later had a lot of issues for the company's strategic plans due to several bugs and flexibility issues embedded in the original design. In the hackathon, I led a team to rewrite it in a weekend, in a way that enables backward compatibility for old coupons issued by the legacy system.
- Building [an anti-phishing system](/2025-06-04/how-i-built-an-anti-phishing-system) that could detect and take down phishing sites within one hour after their initial deployment.
- Recovering production database table records lost during a MySQL live schema migration performed by another engineering personnel using [SoundCloud's Large Hadron Migrator](https://github.com/soundcloud/lhm) without having the table's backup due to a failing DB backup scheduled job. I basically collected bits of data I could find about the missing records from what remained in the system, and then madee a script to recreate the missing records according to the application logic used when processing the missing records' data. The resulting recreated records weren't exact copies of the missing records, as there were two fields I couldn't recreate from the bits of data I could find, but it worked for us (I presented this at an ID-Ruby meetup back in 2016).

For a period in 2014-2015, I was also pursuing a master's degree at Swiss German University (SGU) on the side, where I learned more about cybersecurity, digital forensics, IT governance, and data mining. My last one year at Bukalapak was spent as a security engineer instead of a software engineer, but I primarily focused on application security by hardening the application and making modifications to Ruby on Rails framework's behavior in our context in order to make it more secure for other devs to build upon.

In the later years, I found my job at Bukalapak a bit repetitive and not as exciting as it used to be. I tried exploring other ongoing projects in the company, some written with other programming languages such as Scala, Elixir, and Lua, but none of them really worked for me. I was thinking that I might benefit from doing more infrastructure-related work, considering the projects I found most exciting for me during my time there were things that required either fast coding speed or deeper infrastructure knowledge.

I was already a fast coder by the company's standards, but I thought I could still improve in the infrastructure side of things. Given that I didn't get much opportunity working with infrastructure at Bukalapak and I already contributed to many parts of their product modules (especially in the earlier versions of those modules), I decided to look for a job somewhere else where I could focus on deepening my lower-level systems knowledge and challenge myself in a new domain.

I joined Bukalapak in 2013 as a fresh graduate, a few months before they started their hypergrowth phase, and I left the company in 2017 as their top code contributor, a few months before they became Indonesia's fourth unicorn following companies like Traveloka, Gojek, and Tokopedia. So I consider it a pretty good run for me overall, and I was there at the perfect moment during their crucial business growth phases.

# Cermati Fintech Group

Late in May 2017, I joined Cermati as a senior software engineer to work on some of their more infrastructure-heavy software engineering projects. I was initially hired to work on [a VoIP systems project](https://medium.com/cermati-tech/webrtc-integrating-cermatis-crm-system-with-telephony-infrastructure-2515ce43ff62) to improve the company's VoIP infrastructure integration with their Customer Relation Management (CRM) system and data analytics infrastructure. But later, I was primarily assigned to lead and manage the company's infrastructure platform engineering team, and also to build a security engineering team from scratch.

Given that I was asked to write Medium articles for Cermati's tech publication starting in 2018, I think my work in Cermati has been pretty accessible publicly from [the company's Medium publication](https://medium.com/cermati-tech) or [my Medium account](https://medium.com/@sdsdkkk). But as with what I did in the previous section about my work at Bukalapak, here are a few interesting things I did at Cermati so far:

- Developing a solution to [integrate their VoIP system with the company's CRM system](https://medium.com/cermati-tech/webrtc-integrating-cermatis-crm-system-with-telephony-infrastructure-2515ce43ff62). A follow-up of this integration was to take the call center analytics dashboard developed by the Head of Product, tweak the implementation to be more proper according to the company's technical standards, and migrate the analytics dashboard to a more proper infrastructure managed by the engineers.
- Created a [Docker-based Ansible testing environment](/2017-06-23/docker-based-ansible-testing-environment) that our engineers can use to test their Ansible playbooks before applying it to real production machines. I was tasked to work with the cloud infrastructure, and I was given a bunch of Ansible playbooks written by our engineers. I needed a way to easily test it in a test environment that can be easily reset after every execution, so I came up with the idea to use Docker containers for the task. Soon after that, I eventually became their first official infrastructure engineer, so nobody else but me really used the testing environment. About a year later, we got my first infrastructure engineer teammate, but I think he didn't really use that environment either. But it's still something I used for a while for experimenting with Ansible and testing new playbooks I made.
- Developing [a CLI tooling framework and simple package management system](https://medium.com/cermati-tech/bcl-cermatis-cli-tooling-framework-and-simple-package-management-for-development-tools-5e5e3143969d) for developing Cermati's internal tools. I named the framework [BCL](https://github.com/cermati/bcl).
- Improving the reliability of our [physical VoIP infrastructure](https://medium.com/cermati-tech/working-with-a-physical-voip-infrastructure-as-a-software-engineer-73892c60c8ce) setup by creating automated scripts to allow the standard setups for the existing VoIP machines to be reliably replicated on newly-provisioned VoIP machines.
- Creating a Raspberry Pi-based hardware solution as an alternative to our UPS brand's official automatic power-off orchestrator to automatically turn off machines when the building is experiencing a power outage and the UPS is nearing its limits for providing backup power. Our UPS brand's official solution required us to buy an expensive extra hardware module that must be installed on each UPS unit, and it had a software agent that was quite complicated to set up and must be set up on every VoIP server machine by our VoIP sysadmins. So I set up a solution that required only one Raspberry Pi which we used as a sensor to detect if the building is experiencing power outage, and a bash script that we need to set up on every VoIP server machines to check with the Raspberry Pi if there is a long enough power outage that the VoIP servers needed to be shut down (and automatically shut it down).
- Designing an [IAM system](https://medium.com/cermati-tech/centralized-cloud-identity-and-access-management-architecture-at-cermati-8d5f4ce4c0aa) and developing its core component, called [IAMX](https://github.com/cermati/iamx). At this point, I was already heavier in management stuff than hands-on stuff, so the heavy lifting of the development to finish the system was done by my team members instead.
- Coming up with an idea of an internal phishing toolkit called Phisherman to enable us to perform [phishing campaigns](https://medium.com/cermati-tech/adopting-an-offensive-approach-towards-raising-security-awareness-d0167636e3c0) with more flexibility. I didn't get to do hands-on work on Phisherman's implementation, but I'm happy that one of our security engineers could implement it well enough that it has been used for every single phishing-related offensive engagement we've been having ever since.

There are other things I did, such as cloud infrastructure migration, leased line setup, and rearchitecting our cloud VPC for PCI-DSS compliance. And since starting in 2018, I was assigned to build and manage several teams, many of the later projects weren't actually done by me myself, so the credit should really go to the team instead.

While I think I've been doing a management job way better than I initially expected, I somewhat miss the sense of satisfaction as an engineer when I managed to come up with a solution and deliver it myself (and then I can claim the achievement). As a manager, it feels a bit rude to claim an achievement the team achieved, even if it started from your idea, since you're probably not the one who worked the hardest on it.

# Reflection

Throughout my career, I was often confused when people asked me what I wanted to achieve. It's a bit ironic because as a manager, that's the exact question I always ask my team members and the candidates I interviewed in order to understand how I should work with them and align their goals with the team's and the company's goals.

The thing is, I mostly made my critical professional decisions in moments when:

- I'm interested in trying something new, and the new project looks interesting enough.
- I need to step up and take more responsibility because the people around me (in this case, the company or the team) need it.

So I can't really say I'm someone with a long-term vision regarding my career trajectory, as my personal decision-making workflow really depends on what I think makes the most sense at a given moment. When there are multiple possible decisions that equally make sense, I'm going to just pick the one I find the most interesting.

I'd say the only areas I never really managed to get the hang of are front-end and mobile development. Despite having developed games in my university years, I quickly learned that I have no patience in making my games look good. This extends to my product development work, as I always needed a front-end engineer in the team to turn the half-assed, messy UI I implemented into something presentable to the users (unless they've provided me with all the standardized components and templates I could just reuse).

While I have been working in the infrastructure domain for the last 8 years, I'm still quite reluctant to really call myself an infrastructure specialist. This is due to the title of infrastructure engineer being used quite broadly; everybody from network engineers, sysadmins, cloud engineers, SREs, DevOps engineers, and platform engineers can claim that title. And I found that the terms like cloud engineer, SRE, DevOps engineer, and platform engineer are also used liberally in many situations, so I'm a bit afraid to set the wrong expectations for people when identifying myself as one.

I'm definitely [not a properly specialized security engineer](/2025-06-09/you-shouldnt-learn-security-the-way-i-did). I can do some security engineering work, and people I know tend to think that I'm somewhat an expert in cybersecurity. But I think I'm more of a software engineer who happened to know some cybersecurity due to having developed an interest in it relatively early and had some mischievous hacking streaks when I was younger.

Also, I don't really prefer infrastructure over product development. I just happened to be interested in improving my infrastructure engineering capabilities at one point and ended up being the first infrastructure engineer of the team, and later on (reluctantly) becoming the team's manager. If at some point I get offered to work on a product development project as an individual contributor, I think I'll take it if my personal interest is taking the turn to go for more product-oriented projects at that point.

So if I have to summarize what I am and which direction I'm planning to go in a short sentence for a professional introduction, I might be struggling with that despite being a relatively senior tech worker at this point.

This is something I consider my biggest personal weakness, as I have been lacking a sense of a greater purpose at work. Many of the people I consider to be great in this field are the kind of people who have a greater purpose they're trying to serve or a certain subdomain in the field they're trying to be a master of.

When I was still early in my career, probably up to 5 years into it, people probably could brush off my lack of long-term direction and purpose as me still trying to discover myself in the exploration phase. But now I'm already 12 years into my career, and for me, it seems like exploring and looking for exciting things to do are all I want.

At least I noticed some differences between me and a few other people I know, whose primary purpose is also in getting excitement from work, and that is my tolerance for stressful situations and relatively boring work. After all, when at work, I have the mindset of, "I do what I'm paid for. If it's what I'm paid for, then so be it."

That probably allowed me to have relatively long tenures at the companies I've worked for so far, almost 4 years back at Bukalapak and already more than 8 years at Cermati. My guess, that probably what got me assigned to be a manager also.

# Conclusion

If there's one thing from my experience so far that might be useful for aspiring software engineers, I'd say it doesn't really matter how you start as long as you develop the right skills for performing the job. I didn't know any Ruby on Rails when I started my first software engineering job (which used Ruby on Rails), but looking back, I totally crushed it there. I didn't start as an infrastructure engineer, but now I've been doing infrastructure work for 8 years already, and so far I think I'm doing relatively well.

I started programming in high school with Visual FoxPro, a language and development tool set I never heard about again after I graduated high school. Nowadays I can code in various other programming languages. So it doesn't really matter what programming language we start with, but choosing a programming language that's more mature for the domain you want to work with definitely will help you get there faster. For example, if you want to be a front-end developer you probably have an advantage if you start with JavaScript, and if you want to be a data scientist you probably have an advantage if you start with Python.

I've met people who think that being able to do more than one thing well is unrealistic. I often saw them quoting Bruce Lee as follows.

> "I fear not the man who has practiced 10,000 kicks once, but I fear the man who has practiced one kick 10,000 times."  
> — Bruce Lee

The thing is, in that quote, Bruce Lee said that you'll be way more effective using one kicking technique you have practiced 10,000 times than using the 10,000 kicking techniques you've only tried doing once. He's definitely not saying that you only need to know just one kick. When he was alive, Bruce Lee must have had more than a few techniques he had trained extensively and could use effectively.

So, try to expand your knowledge and skill set as much as possible. But make sure you do it in a way you can enjoy.

# References

[Nothing You Learn is Useless](/2025-01-03/nothing-you-learn-is-useless)

[You Shouldn't Learn Security the Way I Did](/2025-06-09/you-shouldnt-learn-security-the-way-i-did)

[Accidental Software Engineer](/2015-09-05/accidental-software-engineer)

[On the Transition from Being an Individual Contributor to Being a People Manager](https://medium.com/cermati-tech/on-the-transition-from-being-an-individual-contributor-to-being-a-people-manager-af91282ab149)

[Dev-C++ Official Website](https://www.bloodshed.net/)

[Diki Andeas](https://www.goodreads.com/author/show/3481230.Diki_Andeas)

[Tifatul Sembiring](https://en.wikipedia.org/wiki/Tifatul_Sembiring)

[RailsCasts Retrospective Part 1: The Fuel](https://rbates.dev/railscasts-retrospective-part-1-the-fuel)

[How I Built an Anti-Phishing System](/2025-06-04/how-i-built-an-anti-phishing-system)

[soundcloud/lhm: Online MySQL Schema Migration](https://github.com/soundcloud/lhm)

[WebRTC: Integrating Cermati’s CRM System with Telephony Infrastructure](https://medium.com/cermati-tech/webrtc-integrating-cermatis-crm-system-with-telephony-infrastructure-2515ce43ff62)

[Cermati Group Tech Blog – Medium](https://medium.com/cermati-tech)

[Edwin Tunggawan – Medium](https://medium.com/@sdsdkkk)

[Docker-Based Ansible Testing Environment](/2017-06-23/docker-based-ansible-testing-environment)

[BCL: Cermati’s CLI Tooling Framework and Simple Package Management for Development Tools](https://medium.com/cermati-tech/bcl-cermatis-cli-tooling-framework-and-simple-package-management-for-development-tools-5e5e3143969d)

[cermati/bcl: A derivation of SierraSoftworks' Bash CLI](https://github.com/cermati/bcl)

[Working with a Physical VoIP Infrastructure as a Software Engineer](https://medium.com/cermati-tech/working-with-a-physical-voip-infrastructure-as-a-software-engineer-73892c60c8ce)

[Centralized Cloud Identity and Access Management Architecture at Cermati](https://medium.com/cermati-tech/centralized-cloud-identity-and-access-management-architecture-at-cermati-8d5f4ce4c0aa)

[Adopting an Offensive Approach Towards Raising Security Awareness](https://medium.com/cermati-tech/adopting-an-offensive-approach-towards-raising-security-awareness-d0167636e3c0)
