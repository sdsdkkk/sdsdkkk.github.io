---
layout: post
title: "System Design, Culture, and Philosophy"
date: 2022-12-11 00:00:00
description: Designing a system to support an organization for a long term
tags: SoftwareEngineering Random
---

Since 2017, I've been working at Cermati mainly in the domain of infrastructure platform engineering. I've written quite a bit about what I and my team at Cermati have been working on, mainly on [Medium](https://medium.com/@sdsdkkk) for [Cermati's Medium publication](https://medium.com/cermati-tech). Since 2020, I also have been building Cermati's cybersecurity team.

Prior to that, I worked at Bukalapak from 2013 to 2017 as a software engineer for various product development efforts and then later as an application security engineer. I switched to the application security role with the advantage of being the top contributor in their codebase, which allows me to dig into a lot of the application logic quite effortlessly.

Having been working in different areas at different stages in my career so far, my views regarding a lot of issues evolve quite a lot over time. What I'm writing in this article reflects my views as per December 2022.

# System Design and Organizational Culture

When someone works as a junior software engineer, they probably don't think a lot about the design decisions and they'll probably rely a lot on the decisions made by their superiors instead. As they gain experience, assuming they grow properly, they should gain enough real life knowledge to start making their own technical system decisions.

One thing that might be overlooked when they start developing their system design skills is that system design is more than just a technical matter. How a system is designed in an organization must be aligned with how the organization breaks down its business domains and how they're all operating towards their common goals.

Suppose we're trying to build a startup, where we're still at the very early stage trying to launch our MVP. We're given the authority to make the technical decisions over how the product is going to be built and how the builds are going to be delivered to the users. What should we do?

First, we must see how much work we're going to do and how many people are in the development team. Let's say that we probably have 3 months to build our first MVP for market validation and we currently have just one developer other than ourselves as the CTO of this startup. The MVP will be focused on implementing a few core features of our product, which is expected to be quickly evolving over time during our market validation phase. We have budget to hire up to 5 more developers.

Given the simplicity of the product we're trying to build and the manpower we have, it's quite obvious that we should use the simplest possible architecture we can come up with. We don't really need to worry about scaling at this point because the market is practically nonexistent yet and it's unlikely that we're going to market the MVP to a huge mass of people during our initial launch.

What we might want to do for the architecture:

- Monolithic MVC web application
- A database, probably Postgres or MySQL if we prefer using SQL databases or MongoDB if we prefer NoSQL
- One VM for web application deployment, one VM for database server
- Assign at least one or two people in the team as deployers

Using microservices and fancier deployment setup such as Kubernetes at this point might not worth the investment, since it takes time and requires more people to set up and maintain. The investment is better directed to the completion of the MVP and iterating the product development to better fit into the market after the MVP launch.

Sounds simple enough? Yes, it is actually that simple in this stage.

Suppose we manage to hire 5 very good engineers as extra manpower during this 3 months, isn't there anything we can do to improve the architecture? We probably can, but I probably wouldn't want to hire 5 engineers at this point if we can do with just hiring 1-2 more engineers. The reason is that we don't even know if the product will actually work yet and how much more work we need to do to improve the MVP later. Worst case, we're hiring them only to build a failed product and everything we built will be disposed in the next 6 months or so.

Someone I personally know was acting as a CTO at one startup and then later holding a VP of Engineering position at another startup. He started out by hiring quite a lot of software engineers for product development along with a few engineers specifically for handling the infrastructure, and he decided to develop everything as microservices from the start.

The result:

- The first startup never really took off, they got the MVP working but there was an issue with their investor so they couldn't continue operating since the funding was cut off.
- The second startup took off and operating for a while, but eventually they needed to lay off almost all of their product and tech team because the tech was too costly to develop and maintain while the revenue generated from the system didn't seem to worth all the cost due to the unnecessary cost and complexity added to the development flow, especially after the COVID-19 pandemic hits.

The fancier stuff such as breaking down the product into microservices, investing in nicer infrastructure setup, and optimizing performance should at least wait until the product is stable enough and it makes sense from the business perspective to invest more in the product and technology development. This friend of mine invested in those stuff way too early.

Now, suppose we have reached a point where it actually makes sense for us to invest in the fancier stuff. What should we do next?

Here's where the organization's culture and values should be the guiding principles in our decisions.

Suppose our organization has strong people managers in our tech department who can direct and organize a large team but these managers aren't really the deep technical knowledge type of people. The organization might be more comfortable in setting a low-medium hiring bar to get more worker to scale the product development and infrastructure operations process to meet business growth demand. Especially if the business needs to grow very fast.

In this case, since the organization is probably going to grow huge very quickly, we might want to focus the effort to break down the business domains into a set of microservices very soon in order to allow the development process to move smoothly even with all of the additional people joining in a very fast pace. The organization might not have that much time to set up good automated deployment flow very quickly, so they might want to hire infrastructure engineers that will act mainly as infrastructure operators to support the product development engineers in their deployment and production troubleshooting needs, since without a good infrastructure governance system it might pose a very high risk to the organization to give the developers infrastructure access.

But in the case where our organization has very technical managers who're relatively weak in managing and directing a large team, we might want to focus on hiring for fewer engineers but with higher hiring bar. This setup might not be ideal if the business needs to grow very fast in a short time, so alignments should be made between business, product, and tech regarding how to meet the company's strategic goals.

In this case, since we have way fewer people and the development team doesn't grow as fast, we can afford to keep the monolithic setup for our system for a bit longer. Since in this scenario we have people who're stronger technically, we have what it takes to invest more in developing better infrastructure, developer platforms, and components that will allow us to develop and launch new products faster with lower cost in the long term. This is something that's relatively easier to do with leaner tech team. Once the team members are more mature, we should be able to quickly hire more people and have the existing team members who has been accustomed to the internal platforms and architecture promoted to lead the newly hired people.

Once we have gone past the MVP stage, how the system should be designed must follow what the organization has and what the organization can afford when pursuing their business goals. While there's no simple right or wrong answer here, there should be an optimal trade-off that needs to be made for the business. The key here is to support the business while playing to the strengths of of the people we have in the team.

# System Design and Philosophy

As with the organization's business goals and culture, the system should be designed with a particular philosophy in mind in order to have a consistency in it. The core philosophy of the system design will be acting as the north star guiding principle for the organization when developing their system architecture.

The core philosophy should not work against the organization's business goals, so in the case where there's an urgent need for the business to move forward we might need to compromise the core philosophy a bit or try to find a temporary workaround to resolve the situation. But a proper resolution will need to be attempted in order to keep the consistency of the system and not break the expectations of the system's designers, developers, and users.

An example of a core philosophy when designing an infrastructure system is as follows:

- Simple to develop, operate, and maintain
- Can be operated with a very small team
- Put little load on any individual team members for business as usual operations
- Avoid unnecessary work
- Make it secure

While this might seem like something that's obvious to pursue, without this guiding principles set as the expectation for the team we might have difficulties enforcing it, especially when we're under pressure from the other teams to serve them and they demand for us to abandon the principle in order to help them achieve their short-term goals.

With this set of guiding principles, at the very least we can be consistent when communicating our goals to the other teams so even when the compromise needs to be made, they're fully aware that we're doing it at the cost of a technical debt that we'll need to fix some time in the future. This will help with the communication.

A product development team might work under a slightly different set of core ideas for their philosophy:

- Build and ship fast
- Ensure stability, reliability, and security of the builds
- Aim for logarithmic computing resource cost growth relative to user growth
- Monitor everything

For a product development team in a startup, fast iteration in building and shipping their software builds is usually the core concern and might need to be prioritized over reliability (under assumption that any faulty builds can be detected and fixed soon enough). While for the infrastructure team, speed in iteration might not be as necessary if compared to reliability due to the infrastructure team isn't building something that the business needs to launch to the market ahead of the competitors, but the infrastructure team needs to ensure the reliability of the platform they're developing and maintaining in order to ensure the product development team's development flow is as efficient as possible.

It is important for the interconnected teams to define their core philosophies and set expectations with one another to ensure the system that's going to be designed will function correctly to support their goals.

# Conclusion

In this article, I'm focusing on the organizational aspects of system design. This is something that I used to overlook a lot when I was still mainly working as a product development engineer, because the company I was working for put most of their resources into fast growth in terms of product launches, traffic, user, and transaction numbers so I was focusing mainly on delivering fast and optimizing for code performance in high traffic situations with the pre-existing technology stack and infrastructure.

I gained a broader view and hands-on experience into how system design is closely intertwined with the company's culture and long term trajectory when I switched companies and got reassigned to developing the company's core infrastructure. Choosing a wrong architecture and overall system workflow might kill the tech organization or even the company, as with the case I mentioned about a friend who decided to use microservices architecture when building his MVP.

While in his first case the company's failure was mostly due to the investor's problems, they might be able to save the company if they decided to go with a simpler tech architecture and leaner development team. For his second case, while the pandemic had its role, it was quite obvious that the tech architecture and the team structure he was going with weren't the best for the phase his company was at. At his second company, while it worked for a bit longer, there were issues with the tech teams' coordinations because they decomposed the product into microservices before the product reached enough maturity and delivered enough value for him to justify the decisions he made for the tech organization.

If I could say something about the failures in both organizations, the core issue was that he was trying to bring an architecture that worked for his team when he was working at a tech unicorn to early stage startups. This was due to his experience being mostly on shipping products, without much experience on how to build an organization. This might be the exact trap I'd get into if I were in his position before I gained more experience in aligning a company's system architecture design plan with the company's culture, values, and visions.

But could the companies work with the architecture he implemented for both of them? If the situation was different, probably it could work out for him. Honestly, it's hard to tell since even if he implemented the architecture I consider as the "correct" one, I can't really tell if the other factors affecting the companies will work according to what I imagine.

System design is a tricky thing. Unlike simply writing code where there are obvious correct and incorrect ways to solve the problem, and where we can rate the solution based on how well the module performs given the set of expected inputs and the load it needs to handle, there are many constraints in system design that's not strictly technical and we might not be very well-aware of such as the business goals, the company vision, and the team members' personalities and preferences.

# References

[Edwin Tunggawan – Medium](https://medium.com/@sdsdkkk)

[Cermati Group Tech Blog – Medium](https://medium.com/cermati-tech)
