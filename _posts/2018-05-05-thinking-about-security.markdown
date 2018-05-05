---
title:  "Thinking About Security"
date:   2018-05-05 00:00:00
description: A Simple Way to Get How It Works
---

### Background

I was attending a local data science event in my area not too long ago, and met a student who had just graduated and starting his career as an engineer.

After a bit of a chat, he told me that he's interested in computer and information security. But he found it as something hard to perceive, and the security class he took at university doesn't seem to give him much insight on it. I asked what was taught at the class, and he answered, "A lot of things. The CIA triad, mobile apps security, network protocols, and many more."

The classes he took might cover many materials regarding the implementation gotchas part of security, but might miss ensuring the students have a basic understanding of what security is. It's not so strange to miss that, because throghout my bachelor's study the security classes didn't cover the basic question of what security is while throghout my master's study it's explained in a very abstract way in the security classes.

### Computer and Information Security

As the name implies, computer and information security covers security regarding computer systems and information contained in it. According to my knowledge and experience, there are usually two ways to teach these classes.

The first one, focusing on the computer systems part of the security, will focus on how to implement secure systems and test the security of existing systems. These classes usually teach the students about cryptography, web application security, UNIX-like system access modes, access control lists, memory corruption, the technical parts of penetration testing, digital forensics, and so on.

The classes under this category might overwhelm the students, since it covers a wide range of technical areas and many students might not have the knowledge or experience to understand the domains of each given case. The students would then end up thinking that learning security is learning all these unknown cool technology--as I was guilty of, which can't be more wrong.

The second one, focusing on the information flow part of the security, will focus on how to manage the flow of information within an organization and how to perform audits to check whether an organization is governed correctly from information security standpoints. These classes usually teach the students about organizational structure, auditing methodologies, law and legal matters, risk management, threat modelling, and so on.

The classes under this category might be too abstract for students without the knowledge or experience to understand how businesses, organizations, and law enforcement work. For some students--and me back then--taking these classes might feel off because it doesn't feel like technology-related course at all. The material coverage is way too abstract for people without any experience on real-life organizations with proper structures and actual risks.

Both categories of classes equip students with skills necessary to perform real-life tasks related to computer and information security. Both are required, since even as the way they're taught seems to separate the computer systems part and the information flow part of security, in real situation both parts works together side by side. We're not updating the access rule to our database servers just for the sake of it, we might be updating it because the old rule just doesn't work after we reassess our security posture to the new threat model we have.

### The CIA Triad

![CIA Triad](/assets/images/posts/cia-triad.gif)

This CIA (Confidentiality-Integrity-Availability) triad stands at the core of computer and information security. The triad is usually included in the introductory classes for computer and information security, but whether the students get the importance of this triad might depends on how it is explained.

**Confidentiality** means that in the system, both in the computer and organizational context, every actors have access only to certain part of the system or information relevant to their roles. Every subsystem and information can only be read by those authorized to read it as defined in the system. Failure in confidentiality would cause the system to leak out sensitive data to unauthorized parties, which could exploit the system for his own gain.

**Integrity** means that in the system, both in the computer and organizational context, the existing data's trustworthiness is guaranteed. The data can't be tampered, and it can only be updated by authorized actors in the system. Failure in integrity would cause the system's data to be untrustworthy, and any reference to the data could lead to wrong conclusion by the actor due to the data being tampered by unauthorized parties.

**Availability** means that in the system, both in the computer and organizational context, any data or information needed by authorized actors should be accessible whenever they need it. Failure in availability would enable unauthorized parties to perform denial of service attacks to the system, rendering the system resource to be inaccessible by authorized actors who should be able to access the resource.

Notice that the CIA triad requires the system to have confidentiality, integrity, and availability in a general sense. It isn't bound to any specific kind of system, and it is applicable to both digital and analog systems. Any failure to fulfill the CIA triad's requirement on a system would render the system insecure.

Taking the CIA triad to any system we design or analyze, we can evaluate whether the system already provides proper control to ensure the fulfillment of all three points in the CIA triad. The system can be a specific function in a software system, or a specific configuration settings in a server, or a physical placement of sensitive documents in an office building, or a defined SOP in an organization. The physical characteristics and the details of the systems mentioned before are all different, but they all have one thing in common: they all have resources and actors, and the actors' access to the resources needs to be regulated.

For a function in a software system--depending on the context of the software--the resource can be memory addresses, registers, or database records. The actors can be an entity or a user currently triggering the function call.

For a physical placement of sensitive documents in an office building, the resources are the sensitive documents and the information contained in it. The actors are people who can access the documents physically.

Whenever the resources in a system can be read, modified, or rendered unavailable by unauthorized parties, the system is considered insecure. That basic mindset enables us to see things from security standpoint.

### Conclusion

After studying security and working closely to it for a few years, I learned that a lot of it isn't about the technology. It's about correctly analyzing a system's characteristics, modelling the architecture of the system, and identifying threats and vulnerabilities lingering within and around the system.

The system itself doesn't need to be something directly related to information technology. It can be the design of a cupboard, the interaction between a person with their environment, or even our own selves. As long as we have sufficient domain knowledge we can use as a base for analyzing the target system, we can always see where its flaws are, how to improve it, and how to exploit it.