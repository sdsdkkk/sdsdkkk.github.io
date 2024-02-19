---
layout: post
title: "Setting Up a Self-Sustaining System"
date: 2024-02-19 00:00:00
description: Setting up a system to reach an equilibrium of sustainability
tags: SoftwareEngineering Biology
---

Some time in January 2024, I was looking for videos on fish evolution and marine biology on YouTube, and I happened to stumble upon [this YouTube channel](https://www.youtube.com/@FatherFish) by Father Fish.

The Father Fish YouTube channel mainly talks about how to set up our own fish tank in a way that turns it into its own ecosystem, which allows the organisms contained in the fish tank to thrive without requiring any intervention from us. The key to reaching this state is reaching the ecosystem's equilibrium, by making sure the system is set up with everything it needs to maintain a food web that allows population control of the organisms in it while keeping the chemical balance of the water inside the fish tank.

In terms of system design, Father Fish does the correct thing: setting up the system to reach and maintain a certain equilibrium that allows for its self-sustainability. This is a concept known as [ecosystem equilibrium](https://inst.eecs.berkeley.edu/~cs10/labs/cur/programming/simulation/ecosystem-simulation.html).

As a software engineer, I then wonder: is this concept of ecosystem equilibrium applicable to the field of software and systems engineering?

# Ecosystem Equilibrium

Ecosystem equilibrium stability can be seen using a multi-dimensional visualization of predator and prey populations and their changes over time. [This article](https://www.leibniz-zmt.de/en/news-at-zmt/news/news-archive/stable-or-unstable-how-researchers-calculate-the-equilibrium-of-an-ecosystem.html) from the Leibniz Centre for Tropical Marine Research might explain it better.

An ecosystem is basically a system that has parameters including the size of populations of each organism in it, which can also be affected by environmental conditions such as humidity, temperature, salinity of water, and other things that might affect the organisms contained in the ecosystem. Hence, changes in an ecosystem can be modeled as recurring matrix operations where an operation between the system state matrix and the population/environmental changes at time step $$t$$ can be calculated to produce the system state matrix of the time step $$t + 1$$ that can then be put into another operation with the new population/environmental changes matrix of time step $$t + 1$$, which might also be influenced by the operation previously performed for the time step $$t$$.

It can be formulated as follows.

$$
A_{t} = A_{t - 1} B_{t - 1}
$$

Where $$A_{t}$$ and $$A_{t - 1}$$ are the ecosystem parameters at time steps $$t$$ and $$t - 1$$, respectively, and $$B_{t - 1}$$ is the changes applied to the ecosystem at time step $$t - 1$$ which results in the condition $$A_{t}$$ at time step $$t$$.

The same formula can be used for modeling changes in other kinds of systems with many parameters over time.

In the case of fish tanks set up by Father Fish, he sets it up to ensure the system can always stay within its equilibrium without human intervention by creating an ecosystem as natural as possible that works purely by the force of nature at play.

# System Equilibrium and Sustainability

For a system to work, there must be a certain equilibrium for the system to maintain. The equilibrium should happen when all parameters fall nicely together within a certain range, and when the equilibrium is broken the system will eventually fall apart.

In an ecosystem, we need a balance of organism populations and chemical substance compositions within the environment to be able to sustain itself. But how about a software system?

The initial question that came to my mind was: what is required for a software system to be self-sustaining? But after thinking about it for a while, I came to the conclusion that in a modern distributed software environment, it is almost impossible to make software that's self-sustaining due to the software's dependency on a lot of external factors we can't control.

We can make software self-sustaining just like the mini-ecosystems built by Father Fish in his fish tanks, if we can ensure that the environment where the software is deployed is as controlled as possible to minimize any disturbance that comes from outside the system. We can minimize the need for human intervention in the system's operations if we can set it up to eliminate as many human factors as possible in its runtime.

But in a distributed software system where a user can access the service publicly via the Internet, it's hard to make the software self-sustaining without putting significant effort into maintaining the software and scaling the infrastructure. In this case, we can't model the system with just the software and the host running it in mind. We also need to include the incoming traffic, the supporting infrastructure, and the people operating the service and keeping it up in the model calculation.

For modern Internet-based distributed software systems, some of the parameters we might want to consider in the calculation:

- The number of services required for the system to work.
- The volume of traffic the system needs to be prepared to handle.
- The number of write operations in the system.
- The number of read operations in the system.
- The cost of the infrastructure.
- The number of engineers required to maintain the system.
- The total cost of employing the engineers required to maintain the system.
- The amount of capital allocated to run the system.
- The amount of revenue brought by the system.

The list still doesn't cover factors such as third-party services the system depends on, which may complicate things even further.

As much as I want to think of ways to make a system self-sustaining without the intervention of human operators, just by looking at the parameters that need to be maintained in a distributed software system, it is easy to come to the conclusion that we can't make it self-sustaining without a lot of effort from human operators and maintainers unless the system has reached a point of equilibrium at one point with the demand for the system and its dependencies have been staying relatively unchanged.

But from a higher-level perspective where we see the human factors, including the human operators and maintainers, as just cogs in the system instead of someone who stands outside the system, we can make it self-sustaining from the perspective of the system's overseer.

I specifically choose the term "overseer" instead of "manager" or "owner" because the system's manager or owner may prefer to be hands-on with the system and actively experiment with it to improve it further according to their vision, which makes them a part of the system parameter and includes them in the category of the human operators and maintainers. But a system's overseer, whose only role is overseeing the system and who only takes actions to avoid the equilibrium being broken and nothing else, can be considered as standing outside the system and the system should be able to be set up to run without the overseer's involvement in it most of the time.

# Conclusion

In the case of Father Fish, he's trying to set up a self-sustaining system where he's acting just as an overseer instead of having to take an active role in it. In the case of distributed software systems, it is hard to do for the individual contributors except for several types of products and internal tools that can be left relatively unchanged for a very long time after their initial development. It is more feasible for managers to set it up to minimize their involvement in the day-to-day development and operations of the system.

As individual contributors, we can generally still try to design individual components and services we built to be as resilient as possible to environmental and requirement changes to minimize our involvement in its runtime, but it will be hard to design a system with a larger scope to stay operating and growing without our hands-on involvement. Also, it might look bad for an individual contributor to not be very involved in the system's operations and further development.

This hands-off approach tends to be quite appreciated in managers because their performance tends to be measured by the resilience of the system they oversee instead of their hands-on involvement in working with the system directly.

But even then, a manager who works mainly on the core product of the business might be viewed in a more negative light if they're taking this approach, as they might not look like they care about the business and the product. Whereas a manager who works mainly on infrastructure, internal platforms, and business support systems might be appreciated if they can set up the system to work with relatively low maintenance and in a way that allows them to be available to help many other areas of the business as needed without causing any disturbance with the system they oversee.

Whether it is acceptable to set things up in a way that allows us to have little involvement in it and allows it to grow organically without our active involvement highly depends on how the business perceives the system we oversee and our supposed role in it. Things that grow organically tend to be more robust and don't require much intervention from outside for them to work, but might not be as well-designed or as optimized to reach a certain goal that might not be the natural domain of those who're involved in building and maintaining the system.

# References

[Father Fish](https://www.youtube.com/@FatherFish)

[Ecosystem Equilibrium](https://inst.eecs.berkeley.edu/~cs10/labs/cur/programming/simulation/ecosystem-simulation.html)

[Stable or unstable? How researchers calculate the equilibrium of an ecosystem](https://www.leibniz-zmt.de/en/news-at-zmt/news/news-archive/stable-or-unstable-how-researchers-calculate-the-equilibrium-of-an-ecosystem.html)
