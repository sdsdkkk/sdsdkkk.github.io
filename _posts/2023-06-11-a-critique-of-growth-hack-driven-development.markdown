---
layout: post
title: "A Critique of Growth Hack-Driven Development"
date: 2023-06-11 00:00:00
description: Thoughts from ten years of experience
tags: SoftwareEngineering
---

Having been working as a software engineer in Indonesia's tech startup scene since 2013, I observed a lot of things happening in the industry throughout the decade. This covers a few aspects of how the practices changed, starting from how the tech startup boom in Indonesia and Southeast Asia region drives the adoption of more modern software engineering approaches and how everyone's trying to shorten their release cycle in order to iterate faster on their products, which are supported by the kind of products most of us are building: web-based digital products.

The convergence of the majority of modern software products into web-based digital products (sometimes with accompanying desktop and mobile apps, which also communicates with the server via HTTP-based API) allowed software developers to take more risks by releasing faster, catching and fixing defects as they're detected through the system monitoring tools and user reports, as the cost of rolling out new builds of the software to their servers is relatively low.

This newly-gained agility was gladly welcomed by the businesses, as it opened the possibility to explore more product ideas with relatively low starting investment and to quickly drop the ideas or pivot to something else if the ideas don't work. This led us to a period of growth hacking, where the companies were trying out as many ideas as they could think of and seeing which ideas moved the needle for their key metrics.

The growth hacks, when not supported by good technology infrastructure and architecture, will eventually pile up tech debts over time. When not addressed properly, this eventually degrades the quality of their software products. This happened to several companies due to their tech debt payments being overdue, as the funds that should be allocated to paying off the tech debts were reinvested into another round of growth hacking instead.

Luckily, as vanity metrics became somewhat obsolete for the investors to measure the success of startups, the growth hacking craze started to cool off and the companies started to refocus their engineering efforts to build their products and businesses in a more proper way to achieve profitability and sustainability.

There are several facets of the technology that might've been neglected by some businesses during the growth hack craze period that I'd like to highlight.

# Software Architecture

When we initially start a company, it is reasonable for us to build a quick and dirty MVP to validate the product to the market and to get the business going as soon as possible. This is actually a good strategy to take at this point, as I've seen [companies trying to do otherwise](/2022-12-11/system-design-culture-and-philosophy) that ended up badly. But there was this glorification of startups' quick and dirty method of delivering value that led some companies to keep building MVPs over time when trying to add more features and product offerings to their existing products.

There might be a few possible reasons why a company would go to build their products incrementally using the approach of building MVPs over and over again, but one thing that I noticed as a reason for this is the lack of coherent product and business vision from the leaders which leads the company towards the "let's try everything and see what works" kind of mentality.

From a technical perspective, this hurts the product from software architecture standpoints:

- The lack of product and business vision makes it hard for the engineers to envision what their software will need to be capable of up to a few years into the future, which causes problems for them when designing the architecture and future-proofing their software system.
- Adding and removing features (or product verticals) from our product repeatedly over time will definitely leave unwanted smells on the codebase if not handled carefully.
- Incentivizing the software engineers to deliver at high velocity on a day-to-day basis and rewarding them mostly based on the time of delivery and the number of features delivered will reduce their incentives to follow the proper architectural guidelines and code quality standards.

Growth hacking, when overdone, tends to focus on short-term gains over long-term sustainability and coherent vision. The damage done to the technical aspects of the product will catch up to the business at some point and greatly increase the cost of future changes required to keep up with the business strategy.

I personally think that growth hacking makes sense if utilized with care, as it's just another tool in the box for driving business growth. But for that, the architecture of the software product needs to be designed to support the amount of growth hack it's going to experience later.

A good architecture will allow the company to do as much growth hacking as they wish without compromising the code quality and the technical architecture of the software product, which signifies strong technical leadership. But when the technical leaders of the company are not mature enough, they might not know when to say no to the growth hack strategies proposed by the business and their technology development could be compromised.

# Reliability and Security

As a growth hack-focused company focuses their efforts on rolling out the products faster to boost the KPI metrics they're trying to grow, they might lose sight of the other aspects of the system that doesn't directly translate to the KPI metrics growth they're chasing. Two of the critical aspects that might get overlooked are reliability and security.

The "move fast and break things" [motto](https://en.wikipedia.org/wiki/Move_fast_and_break_things) coined by Facebook was adopted by many growing startups. They were trying to test their limits on how fast they can iterate changes and grow their businesses without breaking their systems too much. This of course stands in opposition to reliability and security which aim to not break anything when introducing changes.

The lack of investment in reliability led to frequent outages for some of the companies I noticed, and the lack of care in security led to horrendous development and operational practices regarding the care they put into handling sensitive user data. This led to their brand image degrading due to how often incidents were happening with their systems and how they handled them.

Once they reach the point where they care enough about their brand name to be associated with quality, reliability, and security, they might start addressing these reliability and security issues. But the problem with the growth hack mentality is that we're encouraged to look for a quick fix for everything to score a big win now, but not to put a lot of effort into fixing it properly to truly eliminate the problem in the future if the problem doesn't directly affect the KPI metrics growth we're trying to boost.

This can be an even more complex problem to tackle than the software architecture, as reliability and security also covers the technology operations aspects and the data processing policies of the company. The scope of these aspects covers the properness of the whole company's business operations workflow, where the properness will be hard to achieve if the mentality that's nurtured from the top-down is a low cost, quick win hack mentality.

# Technology Organization Growth

There are two main concerns I'd like to raise with the technology organization growth as I saw with growth hacking-oriented companies:

- Bloated technology organization due to poor organizational and architectural planning
- Headcount as a growth KPI metric to boost

Due to poor prioritization and a lack of coherent vision, the strategy employed by the companies to increase their engineering and technology operations capacity is usually by adding more manpower. In this scenario, they usually have a lot of work to do to boost their KPI metrics but they don't allocate enough time to research and plan how to best approach the situation. The most obvious way to increase the organization's capacity is by adding more headcount to the organization, so they go with it.

Headcount as a growth KPI metric to boost may originate from the need to quickly increase the capacity of the technology organization as the headcount is perceived as the indicator of capacity, but at some companies this eventually turned into its own KPI metric to boost for vanity reasons as employing and managing a lot of people is seen as a status symbol especially during the [zero interest rate](https://en.wikipedia.org/wiki/Zero_interest-rate_policy) era.

The thing is, I believe a lot of work can be eliminated from these companies if they have a coherent vision, prioritize properly, set standards, and actually aim to solve the problems they're dealing with in a proper manner. The recent layoffs in many startup companies during the tech winter might serve as evidence of this, as many companies started closing down the business units and operations they deemed to be more of a liability than an asset.

Just having a coherent vision alone may eliminate so much work that needs to be done if compared to the company basically using a multi-armed bandit strategy with so many arms allocated to business exploration. Add proper prioritization and standard setting there, and we can eliminate the low value work from our to-do list while also reducing our team's cognitive load for decision-making at the micro level as they can just refer to the standards for that.

# Platform Engineering

Growth hack-minded companies tend to allocate most of their resources to projects that immediately boost their business KPI metrics while losing sight of the more subtle strategic projects that will give them a better payoff in the long run. I think quite a lot of the growth hack-oriented companies that I know of are late to start their internal platform development to boost their organization's technological capacity, and probably their focus on short-term quick gains is the reason for it.

Building a good internal platform and toolkit can actually eliminate quite a lot of problems in the long run, covering software architecture, reliability, security, and organizational growth as mentioned in the previous sections. A platform engineering team can start out by building platforms that help with enforcing the standards regarding sofware architecture and security, while also serving to ensure the reliability and security of the infrastructure-level operations of the technology organization.

The infrastructure capability boosts gained from the platform and toolkit may lower the bar required for the whole engineering team to safely perform infrastructure work, which reduces the need for a large dedicated infrastructure operations team and reduces the required headcount for the technology organization to perform. This will help in scaling the infrastructure operations team in the future, by ensuring the team can deliver more value with fewer headcounts.

Investing in building internal tools that are geared towards boosting productivity and improving reliability (also security) might even allow us to cut the required headcount of the development teams by 10-20% to achieve the expected level of productivity for the teams, by automating the manual work that needs to be done on a day-to-day basis for their development purposes and minimizing the occurrence of production incidents that needs to be handled and investigated.

# Conclusion

This post concludes what I think tech companies need to invest in for better sustainability, which may remain unaddressed when growth hack is the game they're playing. I myself am not a marketer nor a marketing technologist, whom I think the growth hack philosophy would work very well for, and I have my own biases against growth hacking as I have seen it doing more harm than good in the technical aspects of the companies I'm aware of so far.

Growth hacking, just as any other tools and methods that can be utilized to help a business, should be employed in moderation according to the needs of the company. Overdoing it will have its own consequences, especially because it aims to gain cheap quick wins which might not be appropriate at all times.

In the technical aspects of a company, I've seen growth hack-driven companies carrying a huge amount of technical and organizational debt due to the short-sightedness of their technology strategies, which then greatly reduces their ability to compete in the market after a few years.

Of course aiming for quick wins isn't bad by itself, as focusing too deeply on the technical aspects and overengineering our technology systems for future big wins while neglecting the present can make a quicker death of the company than growth hacking.
