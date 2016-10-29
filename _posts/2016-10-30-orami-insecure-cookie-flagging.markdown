---
title:  "Orami Insecure Cookie Flagging"
date:   2016-10-30 00:00:00
description: Session Cookie Accessible via JavaScript
---

### Timeline

__October 9, 2016:__ A report is made to Orami regarding the vulnerability on afternoon. Contacted a friend who worked there for the tech email, her superior told her to have me send the report to him. Got a reply from her superior that the issue will be forwarded to the technical team.

__October 13, 2016:__ Noticed that no fix had been made. Sent another email asking my friend's superior to remind the technical team for the fix, especially regarding the PHP session cookie.

__October 30, 2016:__ Looks like there's no fix issued yet. Since it's been three weeks, I decided to write this post.

### The Vulnerability

I was browsing [Orami by Bilna](https://www.orami.co.id/) earlier this month on a fine weekend on October 9th. I noticed that their cookies aren't protected with Secure and HTTPOnly flag.

![Orami PHPSESSID JavaScript Access](/assets/images/posts/orami-phpsessid-js-access.png)

All of the cookies I saw on the web are accessible from JavaScript. But I think the one that needs the most attention is the PHPSESSID cookie.

The PHPSESSID cookie serves as the session identifier in a PHP web application.

### Risks

Having the PHPSESSID accessible via JavaScript would open a session hijacking vulnerability if there's an exploitable XSS vulnerability in the web application.

Session hijacking would allow attackers to hijack any logged in user's session to access Orami using the victim's account and privilege.

The session hijacking will only last as long as the victim's logged in, as Orami isn't vulnerable to session replays.