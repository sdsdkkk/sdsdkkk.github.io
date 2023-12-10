---
layout: post
title: "PPT Framework in Software Engineering"
date: 2023-12-10 00:00:00
description: Identifying people, process, and technology problems
tags: SoftwareEngineering Security
---

[The PPT framework](https://whatfix.com/blog/people-process-technology-framework/) is a framework that originated from the field of management, which is also commonly taught to people working in cybersecurity due to the strategic nature of a lot of security jobs. As a software engineer who's also a cybersecurity practitioner and was receiving formal education in IT security (before cybersecurity became the more popular term).

The PPT framework is used to help an organization build systems that effectively align their people, process, and technology to achieve the organization's goals. In the context of cybersecurity, it's heavily used to improve an organization's security posture by introducing improvements on each front. The following is an example of how the PPT framework is used in cybersecurity:

- **People:** ensure the employees are aware of common security risks and are trained on how to identify and respond to them
- **Process:** ensure proper protocols and approval workflows are implemented in the organization as safeguards to minimize the risks of rogue employees abusing and exploiting the organization's critical assets
- **Technology:** ensure the use of secure technology solutions to enforce security policies in the organization

# PPT Framework in Software Engineering and Cybersecurity

As mentioned in the previous section, I'm a software engineer who also happens to be a cybersecurity practitioner. I actually first learned about the PPT framework when I was pursuing my master's degree in IT with a specialization in IT security, and while it keeps appearing whenever I'm going through cybersecurity materials, the PPT framework rarely appears in materials related to software engineering (even in topics related to software engineering management).

This might be the reason why I noticed that whenever I'm working on software engineering problems with fellow software engineers, many of them (even the more senior ones in their career ladder) seem to be very biased towards framing their problems as technology problems, as opposed to framing it as people or process problems. The rise (and abuse) of terms such as [DevOps](https://www.atlassian.com/devops) and [CI/CD](https://www.synopsys.com/glossary/what-is-cicd.html) might be visible symptoms of this. DevOps and CI/CD were originally terms that were supposed to refer to the implementation of certain processes in the SDLC in order to make the engineering organization more agile and to make the software more adaptable to the ever-changing business requirements, but the software engineering community at large reduced those terms into pieces of technology which, if implemented in their organization, will allow them to become more agile.

On the other hand, as a software engineer, I found that security practitioners (especially those who work in GRC) tend to be very biased towards framing their problems as process problems instead of people or technology problems. I have talked about this in [another post](/2023-08-12/make-compliance-great-again) where I mentioned about compliance people who blindly push the organization to implement a process without understanding the technology architecture that should be governed by the process.

As someone who resides in the intersection of software engineering and cybersecurity, this has put me in awkward situations whenever I need to work with software engineers whose problem is actually a process problem and whenever I need to work with security practitioners whose problem is actually a technology problem. But for this post, I'm going to talk primarily about the software engineering side of things.

# Fixation to Framing Problems as Technology Problems

Software engineers have this tendency to try and frame every problem as a technology problem, which a lot of my non-techie friends have complained about since I was still an undergrad CS student. And I think they're right.

As software engineers, we're encouraged to build software solutions to real-life problems. With the amount of money poured by VCs to the tech (especially software) industry in the past few decades and the rising incentives for people to become software developers, we seem to have come to believe that every problem is just one piece of software away from being solved.

But the problem is that not all problems can be solved by writing code and deploying it to servers and edge devices. The code itself might be able to simulate the problem and the solution, but it doesn't solve the real problem when the problem is residing in a physical plane where the code's logic has no direct interface to interact with the environment. And even if the code's logic could have a direct interface to manipulate the physical world, it might not always be acceptable to allow the code to do that given the possible unforeseen impact it might have on organizations and people's livelihood and well-being.

There are ways to solve problems that don't involve writing software, especially when the scope of the problem is bigger than the environment where the software is deployed.

# People, Process, and Technology of Software Engineering

In software engineering organizations, just as in organizations in general, we need to align the people, process, and technology aspects in order to achieve the organization's strategic goals. But in the context of a software engineering organization, the goals usually involve software delivery and quality as its primary measure of success.

The following is an example of how we can identify which aspect of the PPT framework a problem falls into:

- **People:** knowledge of the tech stack used in the organization, knowledge of the organization's decision-making workflows and code/architectural conventions
- **Process:** the software development life cycle of the organization, the procedures to perform deployments and apply changes to the database, the on-call rotation procedure in the team
- **Technology:** the software development and delivery infrastructure, the architecture of the software being built

The problem with how many people in software engineering perceive agile is that they think of it as a technology problem, whereas it should be approached as a process problem with technology as a supporting component. This is because agile is a class of software development methodologies, which govern over our software development life cycle (which falls under the process domain of the PPT framework).

While the implementation of the software development life cycle processes can be made smoother and more seamless by using the right technology implementation, at its core, it is a process problem for which the organization and leadership must define the process correctly before pushing for implementations in the technology aspect. Otherwise, the effort on the technology front wouldn't yield an optimal result in improving the organization's agility since the process isn't designed properly for the goal the organization's supposed to achieve.

The part where the engineering management misunderstands how to achieve agility in their software development workflow is the people aspect of this problem. But in this specific case, this is a harder people problem to solve because when the leaders are blind to their own misconceptions, it's possible to have nobody that can guide them in the organization.

I think this problem is also quite common in the field of software testing, where many organizations consider test automation as the pinnacle of software testing practices and their software quality issues can be solved by implementing test automation in their testing process. Just as the agile implementation misconceptions I previously mentioned, this is another case where people are putting the technology before the process in a manner analogous to putting the cart before the horse.

# What Software Engineering Managers and Leaders Should Do

A lot of problems in software engineering are actually process problems. Back in 2020-2021 I spent quite a lot of time studying other branches of engineering, such as electrical engineering and mechanical engineering, and tried to incorporate the knowledge I gained from those other engineering branches into the domain of software engineering.

After going through all the engineering disciplines I managed to cover, I personally think that [industrial engineering](https://en.wikipedia.org/wiki/Industrial_engineering) is actually the closest and the most relevant to our modern day-to-day software engineering work. And what does the field of industrial engineering cover? The optimization of processes and organizations in order to yield the most optimal production result.

Electrical and mechanical engineering immediately gave me a better understanding of why software needs to be designed a certain way by visually seeing how electronics and mechanical engines' modularity works and how the components interact with each other. Unlike both of them, industrial engineering was the most technically boring engineering branch I visited during the time due to the focus on organizational process optimization. But it immediately clicked for me that most hard problems in day-to-day software engineering work for most people are more of industrial engineering problems than computer science problems.

So, yeah. My advice to software engineering managers and leaders, try studying some industrial engineering to recalibrate our minds to be able to see more from the process point of view to counterbalance our usual tendencies to see problems from the technology point of view.

# References

[The People, Process, Technology (PPT) Framework](https://whatfix.com/blog/people-process-technology-framework/)

[What is DevOps?](https://www.atlassian.com/devops)

[What is CI/CD and How Does It Work?](https://www.synopsys.com/glossary/what-is-cicd.html)

[Make Compliance Great Again](/2023-08-12/make-compliance-great-again)

[Industrial engineering](https://en.wikipedia.org/wiki/Industrial_engineering)
