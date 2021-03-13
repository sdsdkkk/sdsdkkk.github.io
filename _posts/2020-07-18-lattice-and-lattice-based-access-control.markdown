---
layout: post
title: "Lattice and Lattice-Based Access Control"
date: 2020-07-18 00:00:00
description: Understanding the concept of a lattice and its application in information security context
tags: ComputerScience Security
---

I happened to stumble upon [this article](https://people.eecs.ku.edu/~saiedian/Teaching/Fa10/710/Readings/lattice-access.pdf) by Ravi S. Sandhu from George Mason University. The article was stored in a server run by the Electrical Engineering and Computer Science School of Engineering of the University of Kansas.

During the advent of multi-user computer systems, system architects started to consider the security requirements of computer systems as a single computer can host the data owned by multiple users which can log into the system to perform their activities. Several approaches to model the access control for the users within a computer system have been developed since then, one of which is the lattice-based access control.

The lattice-based access control is based on the mathematical concept of [lattice](https://en.wikipedia.org/wiki/Lattice_(order)).

# Lattice

There are multiple concepts in the field of mathematics called as lattice, and they're quite different. We're talking about [lattice](https://en.wikipedia.org/wiki/Lattice_(order)) within the context of order theory, not [lattice](https://en.wikipedia.org/wiki/Lattice_(group)) in the context of group theory.

For general lattice, the algebraic structure consists of a lattice $$L$$ and two binary operations $$\lor$$ and $$\land$$.

Lattice operations are commutative.

$$
a \lor b = b \lor a \\
a \land b = b \land a
$$

They're also associative.

$$
a \lor (b \lor c) = (a \lor b) \lor c \\
a \land (b \land c) = (a \land b) \land c
$$

The following is the absorption laws for the lattice operations.

$$
a \lor (a \land b) = a \\
a \land (a \lor b) = a
$$

The following is the idempotent laws of the lattice operations.

$$
a \lor a = a \\
a \land a = a
$$

For bounded lattice, the algebraic structure also has elements 0 and 1. The element 0 functions as the lattice's bottom and the identity element for the join operation $$\lor$$, while the element 1 functions as the lattice's top and the identity element for the meet operation $$\land$$. How 0 and 1 serve as an identity to the lattice operations is shown as follows.

$$
a \lor 0 = a \\
a \land 1 = a
$$

The following illustration visualizes how a lattice works..

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (8,0) circle (0.2in) node {0};

    \draw [->] (8,0.5) -- (4,1.5);
    \draw [->] (8,0.5) -- (8,1.5);
    \draw [->] (8,0.5) -- (12,1.5);

    \draw (4,2) circle (0.2in) node {A};
    \draw (8,2) circle (0.2in) node {B};
    \draw (12,2) circle (0.2in) node {C};
    
    \draw [->] (4,2.5) -- (4,3.5);
    \draw [->] (4,2.5) -- (8,3.5);
    \draw [->] (8,2.5) -- (4,3.5);
    \draw [->] (8,2.5) -- (12,3.5);
    \draw [->] (12,2.5) -- (8,3.5);
    \draw [->] (12,2.5) -- (12,3.5);
    
    \draw (4,4) circle (0.2in) node {A, B};
    \draw (8,4) circle (0.2in) node {A, C};
    \draw (12,4) circle (0.2in) node {B, C};

    \draw [->] (4,4.5) -- (8,5.5);
    \draw [->] (8,4.5) -- (8,5.5);
    \draw [->] (12,4.5) -- (8,5.5);
    
    \draw (8,6) circle (0.2in) node {A,B,C};
  \end{tikzpicture}
</script>

# Access Control

In information security, [access control](https://en.wikipedia.org/wiki/Access_control) is the selective restriction of access to a resource. In the implementation, the access control is generally implemented with access control policies and an authorization process.

Access control policy is a set of defined rules stating who has the permission to access a certain resource. Authorization process is a process to verify whether the accessor is allowed to access the requested resource according to the specified access control policies of the system.

Some common implementation of access control are role-based access control and rule-based access control. Both are commonly referred to with the acronym RBAC, so we're going to just use the full name of the access control methods to refer to them instead of the ambiguous acronym.

A role-based access control can be used in a system where there are several roles for the users with different levels of access privileges. This is commonly used in web applications, such as [Internet forums](https://en.wikipedia.org/wiki/Internet_forum). Internet forums generally have three levels of privileges assigned to each member.

| Privilege Level | Permission
|-----------------|----------------------------------------------------------------------------------------------
| Administrator   | Access all resources in the forum, administering the system, and modifying the display.
| Moderator       | Editing contents in the forum, suspending/banning members, and managing subforum structures.
| Member          | Posting messages in the forum and editing/deleting their own messages.

A rule-based access control can be used in a flexible manner, as it can be implemented even in a system without a defined rules on the users' access roles. An example is allowing access to a certain resource only from a list of allowed IP addresses. This rule doesn't specify which users can access the resource, but it specifies how it's allowed to be accessed and anyone who matches the rules implemented for the access policies are allowed to access it regardless of their role or privilege within the system.

# Lattice-Based Access Control

A lattice-based access control is basically a lattice used to model access control policies. Suppose we have a system with the following access rules.

| Privilege Level | Permission
|-----------------|----------------------------------------------------------------------------------------------------------
| User            | Accessing non-critical resources within the system as an unprivileged user.
| Dev             | Accessing development resources and any resources accessible by an unprivileged user.
| Ops             | Accessing ops monitoring resources and any resources accessible by an unprivileged user.
| Sec             | Accessing security management resources and any resources accessible by an unprivileged user.
| DevOps          | Accessing any resources accessible by a dev staff and any resources accessible by an ops staff.
| DevSec          | Accessing any resources accessible by a dev staff and any resources accessible by a sec staff.
| SecOps          | Accessing any resources accessible by a sec staff and any resources accessible by an ops staff.
| Admin           | Accessing any resources accessible by a dev staff, any resources accessible by an ops staff, and any resources accessible by a sec staff.

The lattice an be modeled as follows.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (8,0) circle (0.2in) node {User};

    \draw [->] (8,0.5) -- (4,1.5);
    \draw [->] (8,0.5) -- (8,1.5);
    \draw [->] (8,0.5) -- (12,1.5);

    \draw (4,2) circle (0.2in) node {Dev};
    \draw (8,2) circle (0.2in) node {Ops};
    \draw (12,2) circle (0.2in) node {Sec};
    
    \draw [->] (4,2.5) -- (4,3.5);
    \draw [->] (4,2.5) -- (8,3.5);
    \draw [->] (8,2.5) -- (4,3.5);
    \draw [->] (8,2.5) -- (12,3.5);
    \draw [->] (12,2.5) -- (8,3.5);
    \draw [->] (12,2.5) -- (12,3.5);
    
    \draw (4,4) circle (0.2in) node {DevOps};
    \draw (8,4) circle (0.2in) node {DevSec};
    \draw (12,4) circle (0.2in) node {SecOps};

    \draw [->] (4,4.5) -- (8,5.5);
    \draw [->] (8,4.5) -- (8,5.5);
    \draw [->] (12,4.5) -- (8,5.5);
    
    \draw (8,6) circle (0.2in) node {Admin};
  \end{tikzpicture}
</script>

The definitions of the access policies can be defined as follows.

$$
Dev \lor Ops = DevOps \\
Dev \lor Sec = DevSec \\
Sec \lor Ops = SecOps
$$

The access level of an unprivileged user is the lowest privilege level which is the bottom of the lattice.

$$
Dev \lor User = Dev \\
Ops \lor User = Ops \\
Sec \lor User = Sec
$$

Since the admin has the privilege of all kinds of users combined, we can state it as follows as the admin role is at the top of the lattice.

$$
Dev \land Admin = Dev \\
Ops \land Admin = Ops \\
Sec \land Admin = Sec
$$

For the DevOps, DevSec, and SecOps policies the relation between them can be defined as follows.

$$
DevOps \land SecOps = Ops \\
DevSec \land SecOps = Sec \\
DevOps \land DevSec = Dev
$$

We can also state the relation between the dev, ops, and sec policies with the following.

$$
Dev \land Ops = User \\
Dev \land Sec = User \\
Sec \land Ops = User
$$

The admin privilege access policy covers the policy for the DevOps, DevSec, and SecOps policies.

$$
DevOps \lor DevSec \lor SecOps = Admin \\
(Dev \lor Ops) \lor (Dev \lor Sec) \lor (Sec \lor Ops) = Admin \\
(Dev \lor Dev) \lor (Ops \lor Sec) \lor (Sec \lor Ops) = Admin \\
(Dev \lor Dev) \lor (Ops \lor Ops) \lor (Sec \lor Sec) = Admin \\
Dev \lor Ops \lor Sec = Admin
$$

Therefore, we can say that the admin privilege permission covers the policies owned by the dev, ops, and sec privileges.

# References

[Lattice-Based Access Control Models](https://people.eecs.ku.edu/~saiedian/Teaching/Fa10/710/Readings/lattice-access.pdf)

[Lattice (order)](https://en.wikipedia.org/wiki/Lattice_(order))

[Access control](https://en.wikipedia.org/wiki/Access_control)

[Internet forum](https://en.wikipedia.org/wiki/Internet_forum)
