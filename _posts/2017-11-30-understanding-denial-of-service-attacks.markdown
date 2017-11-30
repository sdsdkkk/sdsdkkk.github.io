---
title:  "Understanding Denial of Service Attacks"
date:   2017-11-30 00:00:00
description: What Makes a DoS Attack a DoS Attack
---

### Background

Some time ago I had a discussion with an acquaintance who's working as a software developer regarding denial of service attacks. I also happens to be a silent reader in a few Indonesian tech-related community discussions, mostly related to software engineering, system administration, and information security.

From what I see, some people (even those in tech industry) seem to have a very narrow understanding regarding denial of service attacks, and also its even more evil twin, the distributed denial of service attacks. By the way, we're going to refer at those attacks respectively as DoS and DDoS to let me save some keystrokes on the writing of this post.

Some people consider them only as network layer attacks by flooding the target system's network bandwidth with packets to clog the network. While it is a correct example of DoS/DDoS, it's not all of it. The purpose of this post is to give a broader view regarding DoS and DDoS attacks in the context of computer systems.

### Defining DoS and DDoS Attacks

Wikipedia[1] gives us a good explanation regarding both attacks, which is spot on.

> In computing, a denial-of-service attack (DoS attack) is a cyber-attack where the perpetrator seeks to make a machine or network resource unavailable to its intended users by temporarily or indefinitely disrupting services of a host connected to the Internet. Denial of service is typically accomplished by flooding the targeted machine or resource with superfluous requests in an attempt to overload systems and prevent some or all legitimate requests from being fulfilled.

> In a distributed denial-of-service attack (DDoS attack), the incoming traffic flooding the victim originates from many different sources. This effectively makes it impossible to stop the attack simply by blocking a single source.

Wikipedia contributors already explained it pretty well. Let's focus on the key points on those two paragraphs.

According to the first Wikipedia paragraph regarding DoS, the perpetrator seeks to make a **machine** or **network resource** unavailable to its intended users. On the paragraph about DDoS attack, it's said that the incoming traffic flooding the victim **originates from many different sources**.

So we can say that a DoS attack is an attack performed with the intention to render a system unavailable to its intended users, and a DDoS attack is a DoS attack that's using a lot of different network resources for delivering the intended attack to the target system.

### How DoS and DDoS Attacks Work

![Service Accessed by Clients from Internet](/assets/images/posts/dos-ddos-network.png)

The picture above shows a service, hosted by a group of servers, are accessed by multiple clients via the Internet. Let's say the service is running on the following system specifications.

- Total network bandwidth of 4Gbps to connect to the Internet.
- Apache HTTP server, forwarding traffic to application server.
- Application server, running older version of Rails with 8GB of memory and no swap space.
- Rails, runing on old Ruby implementation with garbage collector ignoring Ruby symbols.
- MySQL as the database server for application.

From the specifications as mentioned above, we can identify several limitations regarding the system's resources with some rough estimations.

- It can't handle more than 4Gbps traffic to the site.
- Apache HTTP server possibly vulnerable against Slowloris[2].
- Application server can't handle more than 8GB of in-memory data.
- Possible mishandling of Ruby symbols might lead to memory leaks.
- Mismanagement of MySQL indices might lead to slow queries.

All five points can be targeted by an attacker to perform DoS/DDoS attacks to exhaust the system's resources and bring the system down.

#### Network Bandwidth

The site would be rendered inaccessible by the legit users if the attacker can flood the network link between the service and the Internet.

The bandwith is 4Gbps. If the attacker has enough resources to eat up the bandwidth with his network packets, he can consume a huge portion of the bandwidth and leave the legit users unable to access the service due to the network being overloaded.

The network resource exhaustion is possibly the most well-known type of DoS/DDoS attacks. There are several ways to perform this attack, starting from mass usage of Low Orbit Ion Cannon[3] to performing NTP amplification attacks[4] using spoofed IP addresses.

#### Apache HTTP Server and Slowloris

Slowloris is a low-bandwidth DoS/DDoS attack that eats up web server resources by opening as many connections as possible to the server until the server can't open any more connection for the legit users.

Apache HTTP server is among the web servers that don't handle their open connections very well and can be exploited with Slowloris if not properly configured. Depending on how many open connections the Apache HTTP server can handle, the attacker might be able to take down the web server with only one host. 

#### Application Server Memory

The application will need to allocate memory for loading objects and functions to perform the task as requested by the user. If some of the web endpoints happen to be not-so-memory-efficient, given that the application server only has 8GB of total memory, having those endpoints continuously getting hits might eat up the memory and leaving the application server unable to allocate any more memory for next incoming requests.

Of course we haven't take the next possible exploit into account.

#### Old Ruby Version and Ruby Symbol Mishandling

Ruby symbol used to be objects that's not garbage-collected during Ruby program runtime and could easily lead to memory leaks if not handled properly[5]. In a long-running Ruby process with complex logic such as large-scale Rails applications, the possibility of memory leaks happening due to symbol mishandling might be not so slim.

If the application has these memory leak bugs, the attacker might be able to intentionally cause the applications to keep generating more Ruby symbol objects in the memory until it exhausts the memory capacity.

Luckily, the symbol is already garbage-collected starting on Ruby 2.2[6]. Whoever's running the service in this study case should upgrade, since their Ruby version is already pretty outdated.

#### Database Mismanagement

The service's running on MySQL, and there's a possibility of mismanaged indices on MySQL tables that leads to the queries performed being slow. Having a lot of slow queries performed at the same time would lead to the declining overall performance of the system due to available database connections being occupied to perform the slow queries, and if heavy enough could bring the system down.

There was a slow query bug in Tokopedia that could be triggered intentionally by attackers[7]. It was not due to some indexing mistakes though. Rather, it's due to missing input validation in application's side that allows the attacker to fetch a large amount of data from the database. It can also lead to exhausting application server memory.

By the way, SQL injections can also end with the service being unavailable due to the tables being dropped.

### Conclusion

There are a lot of variations for DoS/DDoS attacks, and not all of them are targeting the network bandwidth of the target system. The goal of DoS/DDoS attacks is to deny the service from being accessed by its intended users, and the method to achieve it could range from exhausting the resource of the system running the service to simply physically turning off the machine running the service.

This post is intended to show several possibilities of performing DoS/DDoS attacks on a system, but the scenarios mentioned in this post is not all of them. Systems with different setups and architectures might not be vulnerable to some of the vulnerabilities mentioned in this post, but open to some other vulnerabilities we haven't think of.

Skilled attackers are pretty creative in performing their attacks, and that's why as system builders, administrators, and defenders it's best if we don't fixate our minds in narrow definitions of security lingos. Otherwise, who knows what we might miss.

### References

[1] [Denial-of-service attack](https://en.wikipedia.org/wiki/Denial-of-service_attack)  
[2] [Slowloris DDoS Attack](https://www.cloudflare.com/learning/ddos/ddos-attack-tools/slowloris/)  
[3] [LOIC (Low Orbit Ion Cannon) - DOS attacking tool](http://resources.infosecinstitute.com/loic-dos-attacking-tool/#gref)  
[4] [NTP Amplification DDoS Attack](https://www.cloudflare.com/learning/ddos/ntp-amplification-ddos-attack/)  
[5] [The Ruby Symbol is a Memory Leak](http://alwayscoding.ca/momentos/2010/12/04/the-ruby-symbol-is-a-memory-leak/)  
[6] [Symbol GC in Ruby 2.2](https://www.sitepoint.com/symbol-gc-ruby-2-2/)  
[7] [Tokopedia DoS Vulnerability](https://sdsdkkk.github.io/2015/tokopedia-dos-vulnerability/)