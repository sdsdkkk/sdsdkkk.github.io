---
layout: post
title: "You Shouldn't Learn Security the Way I Did"
date: 2025-06-09 00:00:00
description: It's kinda illegal to do so
tags: Security Random
---

I've been getting quite a lot of video recommendations on YouTube on how to learn to hack and stuff. And after seeing a few of them, it reminds me of a few occasions when younger security practitioners asked me for some tips in learning to hack according to my experience.

The thing is, with all the cybersecurity learning resources today’s aspiring security engineers have, they have ways to learn cybersecurity concepts and technical fundamentals in ways I couldn't do. And despite having worked to secure various systems so far, I've never really been a career security professional or security engineer. I was formally assigned the title of Security Engineer at Bukalapak at one point but I'd say I was still approaching the role the way a software engineer would, by hardening the application and implementing a few stuff.

I did learn to hack at some point. But I definitely don't consider myself as a specialist in it, and it's a bit hard for me to pinpoint the exact moments I actually learned to do so. It's way easier for me to pinpoint the exact moments I actually found security holes in various systems with whatever knowledge I had at the time.

# High School Years

During high school, I learned about sites such as [Hack This Site](https://www.hackthissite.org/) and tried the challenges for a bit. But I don't think that led me to anything useful.

The more useful stuff I learned might come from when in 2008 (or was it 2009?) I decided to install Ubuntu (I forgot which version) on the Windows XP PC my family had since 2003. Windows XP felt like it became slower over time, with the aging hardware possibly contributing to it. We didn't have the budget for purchasing new hardware, and my high school IT teacher (whom I thought wasn't really that good, just trying to throw some technical lingos to me to get me impressed) told me, "You're not a real IT guy until you use Linux. But it's hard, I warn you."

I first installed Ubuntu using [Wubi](https://en.wikipedia.org/wiki/Wubi_(software)), and after a few more rounds of exploration, I decided to just ditch the Windows XP OS and install Linux Mint the normal way for the family to use.

Prior to ditching Windows XP, I did play around quite a lot with Windows XP's multi-user management and directory permission settings and set up multiple accounts on the PC so each user could have their own personal directories with basic permissions on who could access what files and directories. Previously, my family all used the PC as a single user, with all files and data easily accessible to each other.

During this period, I also learned how to do batch scripting and made a few basic batch script viruses that caused a Windows PC to hang when running. I passed that around at the school's computer lab during classes on a few class sessions, as I learned how to transfer files and exchange messages between Windows machines in the same LAN network remotely.

I also learned some programming with Visual Basic and Visual FoxPro around this time. But I couldn't really make any malware at the time, despite learning to make a more sophisticated virus was my primary reason for getting into it.

I also used to hang around several Internet forums. This was the time when Friendster and MySpace were the leading social networking sites, and using real names on the Internet wasn't the norm. I was involved in some Internet fights and online forum "gangs" at the time.

Using the few bits of information I have on everyone I know from their forum posts and their social media, I managed to find out their school and home addresses from a system owned by Indonesia's Ministry of Education. I learned Google dorking at some point prior to this point in time, and I used Google dorking tricks to find out where I could get Excel files containing their data from the Ministry of Education's public website.

# University Years

During my first year of university at Universitas Multimedia Nusantara (UMN), one of my forum mates told me that another one of our forum mates was enrolled at the same university as me and we were both in the Computer Science program. I never realized that because the guy had been faking his photos in forum posts, and his social media profiles were fake. I was told his real name (not his full name, but his nickname and his last name) and due to this guy having problems with another one of our forum mates (a girl), I was asked to track down this guy in the university.

I found a photo of the guy in one of my major's group pictures with his full name written under his face (added by the lecturer who took the photo so everyone could know each other). But I soon found out that guy only attended classes a few times and then went MIA, and he didn't seem to have any social media accounts under his real name so that was all I could tell my forum mates about the guy.

I got Dr. Hargyo Tri Nugroho Ignatius (not a doctor yet back then) as my lecturer for my Computer and Network Security class back in my fifth semester. I really liked the class, as he used to be in some Indonesian black hat hacker group a few years back and he showed us quite a lot of practical hacking tricks in his classes. He also gave us assignments where we needed to find real vulnerabilities where we could exploit real systems in certain ways. From doing one of his assignments I managed to get admin access to an abandoned system owned by a well-known Indonesian company. He seemed to be acquainted with people like [Jim Geovedi](https://en.wikipedia.org/wiki/Jim_Geovedi) and Juny Maimun (better known as Acong, the founder of Maxindo and Indowebster) from his hacking days, so I was definitely pretty privileged to have gotten him as my lecturer.

He, along with my senior at university, Dian Sandythia (better known as Sandy), who had interests in web application security and had experience exploiting real web applications, formed a security community at the university called INSECxT. I joined this community, and after Sandy showed us that our university's academic information system contained IDOR and session fixation vulnerabilities, I played around with the academic information system and found a method I could use to extract the whole student body's plaintext passwords from the academic information system by chaining the vulnerability.

I found a vulnerability where I could read my own plaintext password provided by the application's backend, and I chained it with the IDOR and session fixation vulnerabilities demonstrated by Sandy to allow me to get the entire student body's passwords. This was pretty bad, so I reported it to the university's IT services department. They put some band-aid solution to prevent me from doing it the exact way I demonstrated to them, but I still could do it after that (in a way that fewer people know of). I always told people to not reuse any of their usual passwords for the university's academic information system after that.

I also used to spend a lot of my free time at the university's library, so I sometimes checked the library's public computers for saved login credentials stored in browsers by previous users of the computers. I never used any of those credentials for malicious purposes, but definitely not something I should be proud of.

I learned about BackTrack Linux (now Kali Linux) and Metasploit from a seminar about computer security and hacking by Dr. Charles Lim (at the time not a doctor yet) from Swiss German University (SGU). I didn't really try it much at this point, but I kept it in mind for later.

During my final year of university, one of the reasons I learned Ruby was because Metasploit was written in Ruby. The other reason was that I was a fan of [Diki Andeas](https://www.goodreads.com/author/show/3481230.Diki_Andeas), the author of Chickenstrip comic strip about a Linux and Ruby on Rails geek Indonesian software developer who wears a chicken costume. Learning Ruby led me to a job at Bukalapak, my first full-time job after finishing my undergraduate thesis.

# Master's Degree

So after I was done with my bachelor's degree, I worked full-time at Bukalapak. I wasn't doing a lot of security-related stuff around this time since I was saving up money to pursue my master's degree at SGU to study computer and network security further since they provided a Master of Information Technology program with a concentration on cybersecurity (the popular term at the time was IT security and governance).

I was especially interested in their Offensive Security course which was taught by Dr. Benfano Soewito (who used to be my academic advisor for my first two semesters at UMN) and their Digital Forensics course which was taught by Dr. Charles Lim (the one who introduced me to BackTrack Linux and Metasploit a few years before).

The courses turned out to be quite a lot easier than I expected, so I was gathering my own learning resources from various sources. I found Carnegie Mellon University's cybersecurity course materials, and writings from [Kim Guldberg](https://rocketreach.co/kim-guldberg-email_4577721) and the late [Adrian Lamo](https://en.wikipedia.org/wiki/Adrian_Lamo) on Quora as my references.

During my time as a master's student at SGU, I managed to find a way to send unauthorized emails from SGU's mail server and spoof the email senders. I reported it to the university and got it fixed.

I also found [a DoS vulnerability at Tokopedia](/2015-09-13/tokopedia-dos-vulnerability) at the time. Leontinus Alpha Edison, the co-founder of Tokopedia, responded to my report personally at the time so I guess it might be as serious as I thought it was. I also started reporting vulnerabilities to a few other companies at the time, but without seeking bounty rewards since I was primarily a software engineer and made my living from that already.

I'm not sure when exactly this happened, but when I was doing my master's study I somehow successfully planted a backdoor in one of the servers owned by a multi-level marketing business where quite a few students from UMN (where I did my bachelor's degree) were recruited into the business.

# Finally Graduating

Only after I graduated with my master's degree in late 2015, an infrastructure engineer at Bukalapak who also had interests in cybersecurity and had participated in several CTF competitions, Franheit Sangapta Manullang, told me about [VulnHub](https://www.vulnhub.com/) and how I could find many VMs I could hack into for learning offensive cybersecurity legally. So I started doing a few VulnHub box challenges every now and then, and wrote writeups of the boxes I solved in this blog (you can see my past posts with "VulnHub ... Writeup" title format).

At this point, I was no longer a student, so I started to be less aggressive in targeting real systems as I could no longer use the "I'm a student" get-out-of-jail card if people were to sue me for the more serious attacks I did. Also, since I already learned about VulnHub, I just used it whenever I was in the mood to take over some systems owned by other people.

I wrote [a Ruby gem called safe_redirect](/2016-07-23/safe-redirect-gem-for-rails) to help secure Rails applications against open redirection vulnerabilities, which was one of the most commonly reported vulnerabilities I saw bug bounty hunters found. I also did some bug hunting for a few companies in my free time, but I never asked for bounty rewards so I never was a bounty hunter.

But one of the few attacks I seriously carried out against real systems after I graduated with my master's degree was [an attack against a friend's web development agency's CMS product](/2016-01-23/a-web-agencys-vulnerable-website). I managed to find a vulnerability I could use to take over two of their registered businesses' websites' admin accounts, and I managed to push it further by figuring out who their clients were and took over some of these clients' websites' admin accounts with the same vulnerability.

I also built [an anti-phishing system](/2025-06-04/how-i-built-an-anti-phishing-system) at Bukalapak where I practically developed and employed malware to take down phishing sites. I did some counteroffensive operations manually against some phishing sites while developing the system.

After resigning from Bukalapak and joining Cermati, I still did a few VulnHub box challenges but generally, I stopped doing offensive works since my interests shifted to something else. I learned a lot of electrical engineering, math, and statistics instead.

But at Cermati, I was assigned to build a security engineering team from scratch and there were a few offensive-leaning operations that the team has been doing under my direction. The more interesting ones are [the internal phishing campaigns](https://medium.com/cermati-tech/adopting-an-offensive-approach-towards-raising-security-awareness-d0167636e3c0) where we use an in-house phishing toolkit we developed ourselves. There are also some [red teaming engagements](https://medium.com/cermati-tech/simulating-a-cyber-attack-against-cermati-fintech-groups-call-center-facilities-594b4b6c9366) we do every now and then, which involve custom-made malware we prepared to avoid antivirus detection and to simulate the kind of attack we want to test our company's resilience against.

# Conclusion

I'm mostly just a software engineer who happens to be a cybersecurity hobbyist and occasionally placed as the cybersecurity guy of the companies I've worked for. Looking at how I started learning cybersecurity and what I've been doing in the field, I didn't exactly learn it in the most optimal, ethical, and legal way for specializing in it either.

Sure, it works for me and it helps the companies I've worked for so far. But I did quite a lot legally questionable stuff when I was learning, which I wouldn't recommend other people doing, as it's possible to unknowingly cause damage through our activities. Also, since I've mostly done it as a hobby or a pastime activity (especially when doing it outside work, and my work primarily hasn't been cybersecurity), there's a very good chance I'm technically lacking in things that should be considered as the standard expected knowledge or skillset of security engineers.

Given that the field of cybersecurity is maturing, and with more resources available for the younger generations who aspire to work in the field to learn it in a legal manner, there are definitely better ways to learn it that don't require people to do what I did (and potentially with better results).

# References

[Hack This Site](https://www.hackthissite.org/)

[Wubi (software)](https://en.wikipedia.org/wiki/Wubi_(software))

[Jim Geovedi](https://en.wikipedia.org/wiki/Jim_Geovedi)

[Diki Andeas](https://www.goodreads.com/author/show/3481230.Diki_Andeas)

[Kim Guldberg](https://rocketreach.co/kim-guldberg-email_4577721)

[Adrian Lamo](https://en.wikipedia.org/wiki/Adrian_Lamo)

[Tokopedia DoS Vulnerability](/2015-09-13/tokopedia-dos-vulnerability)

[Vulnerable by Design ~ VulnHub](https://www.vulnhub.com/)

[Safe Redirect Gem for Rails](/2016-07-23/safe-redirect-gem-for-rails)

[A Web Agency's Vulnerable Website](/2016-01-23/a-web-agencys-vulnerable-website)

[How I Built an Anti-Phishing System](/2025-06-04/how-i-built-an-anti-phishing-system)

[Adopting an Offensive Approach Towards Raising Security Awareness](https://medium.com/cermati-tech/adopting-an-offensive-approach-towards-raising-security-awareness-d0167636e3c0)

[Simulating a Cyber Attack Against Cermati Fintech Group’s Call Center Facilities](https://medium.com/cermati-tech/simulating-a-cyber-attack-against-cermati-fintech-groups-call-center-facilities-594b4b6c9366)
