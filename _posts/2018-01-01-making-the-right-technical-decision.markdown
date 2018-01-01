---
title:  "Making the Right Technical Decision"
date:   2018-01-01 00:00:00
description: Some Thoughts Before Making a Technical Decision
---

### Background

I happened to have met CTOs and VPs of engineering from a few companies, mostly startups, over the course of my engineering career. Some of them are seasoned engineers with years of engineering experience before they founded their companies or before they were recruited to their current companies. Some others were fresh out of college or not didn't have a very solid engineering knowledge when appointed as the leader for the company's engineering/technology department.

One time, a junior engineer asked me a question, "Which CTO from those you have experience with do you think is the better one?"

Naturally, due to the difference in their background and the nature of the organizations they're leading, they have different styles in their decision-making. We can't really compare them and say if one is better than the other, but there are a few things we can look at to see if they're making the right decision for the company.

Another time, a more experienced engineer that was offered a CTO position for a startup also asked a question, "How do you know whether the tech stack you choose is the right one? Which stack is the best?"

Well, I'd say that there's no such thing as the best tech stack. The best we can do might be choosing the right trade-off for the organization's longevity. It depends on a lot of things, such as the core technology of your company, the business process you have, the availability of talent around your area, how much load the system is expected to handle, and how much budget you have.

### Technical Decisions

Technical decisions made in the organization could determine the future flexibility of the product for business expansion, the scalability of the infrastructure, the development speed of the product, and the amount of suffering the ops team shall experience while keeping the whole system up and running.

One of the roles of whoever in charge of the whole organization's technology is to make sound decisions and right trade-offs regarding the organization's tech to ensure the organization's able to achieve their goals, whatever it might be.

Due to the responsibility regarding decision-making, I'd say the seasoned engineers usually handle things more gracefully compared to their fresh graduate counterparts due to them having accumulated more experience that enables them to see and anticipate a few steps further into the future. Though in terms of helping their organizations achieving their goals, both sides has those who succeeded and those who failed.

While the successful ones aren't necessarily the better decision-makers due to the many factors that affects the organization outside the technology, it'd be way more pleasant to those who belong in the department if they're led by a good one.

Here are some examples of cases where a decision needs to be made.

#### Which programming language and framework should we use?

In the last decade, lots of programming languages and frameworks were created and gaining popularity. For a startup company, choosing the right language and framework is critical to deliver the product to the market as soon as possible.

A soon-to-be CTO of a startup yet to be founded couldn't decide whether he should use Golang, JavaScript, PHP, or Python as his back-end development language. He already decided to use React.js for the front-end development standard, but he couldn't seem to decide which language and framework to use for the back-end.

The answer would depend on his familiarity with the language and framework, his confidence to deliver the product using his chosen stack, and his confidence to attract engineers who're skillful and willing to work under him with what the stack.

> Which language suits your product best?  
> Which framework helps you to build your MVP quickly?  
> Do you think the framework is going to stay stable and reliable for the foreseeable future?  
> Do you know where to recruit engineers willing to work with your language and framework?

#### Would it be good for the organization's long term interest?

Let's say we need to add a new killer feature to our product. This feature is expected to raise the company revenue up to 50%, and we're asked to implement this as soon as possible. The problem is, the structure of the product's existing codebase doesn't fit for what we'd like to implement.

If we force ourselves to implement it as a quick hack, it can work as a feature in a few days of work. But the code and the database schema will need to be improperly modified and might hurt our prospect of extending the functionality of affected code in the future for other features.

If we implement the feature properly, it will take about a month of work. The CEO thinks a month is too long for something that can give significant boost on company KPI right now.

What is the right decision? Your call. It depends on the situation your company is in.

> Is your company in a dire financial crisis right now?  
> Will your company survive if the killer feature takes a month to implement?  
> Is the revenue increase prediction reliable?  
> How much is the cost to fix the messed-up codebase and database schema after the hack?

#### Do we really need this new technology?

There's this piece of technology called Kubernetes. It's pretty useful to orchestrate containers on production server, where each container runs a microservice. Each microservice is put into a separate container that contains the dependency required for the microservice to run. Each microservice may require different library dependencies and built using different technology.

Kubernetes allows easy deployment of containers to production, and it allows different teams to use their own tech stack when building their microservices due to the use of containers.

Should we use this on our production environment? It depends on the complexity of the system and how we've been enforcing the engineering standards in the organization. It might be overkill for systems without much variation regarding the tech stack across services, especially if the services aren't that complex.

> How many microservices do you need to orchestrate?  
> Do you need the microservices to be built using different tech stacks?   
> What other requirements are going to be added to the system to deploy it?  
> Does it make the lives of your dev/ops team easier?

### Conclusion

It's hard to say whether a technical decision is right or wrong without understanding the context of the decision. Sometimes it's even difficult to decide whether the decision is good or not given the context, as different people think differently and might have different views regarding the decision.

I met the CTO of a startup who chose CodeIgniter to build his product. According to my knowledge, his chosen framework might not be the most popular one for PHP-based startups nowadays. I also don't think it was a reasonable choice back in the time when he started building his product, but I'm not really in the place to say whether it's a good or a bad decision. While I'd disagree with his tech stack choice, his company stays alive up to the time when I'm writing this post and have yet to run any single company on my own.

The decisions they made might be suboptimal at times. But as with other human beings, technical decision-makers are also in the process of growing and improving themselves. I personally think the best ones are those who understands that they themselves are in the process of growth and improvement, and genuinely try to keep improving.
