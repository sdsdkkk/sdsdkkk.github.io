---
layout: post
title: "How I Built an Anti-Phishing System"
date: 2025-06-04 00:00:00
description: Basically a malware that destroys the phishing site
tags: SoftwareEngineering Security
---

On a few occasions, I've been asked by people what I consider as the coolest thing I've built in my career as a software engineer up to the point the question is asked.

There are a few candidates on which project I consider the coolest. But if we speak in terms of the impact the project had to the regular non-techie Internet users, then the anti-phishing system I built back when I was working at Bukalapak should be up there on the list.

This is a system I rarely talk about in detail, considering that if I disclose the approach I used then the phishers can learn how to disable the trap I set up for them. But considering that [Bukalapak closed down their C2C e-commerce business](https://jakartaglobe.id/business/bukalapak-ceases-most-ecommerce-operations-shifts-to-utility-payments-amid-stock-decline) where I did the entirety of my work when I was there, and how I noticed that the phishing trends have been evolving in the past 10 years, I think disclosing some of the tricks I used when implementing the anti-phishing system now might not impact the security of their current users significantly. It should also be educational for software engineers as a trick they can deploy for securing their systems, or for security engineers when trying to counter some kind of attacks.

# Background

Bukalapak used to be one of the biggest C2C e-commerce sites in Indonesia, and one of the first local e-commerce sites with a built-in escrow service (which I believe Tokopedia had before us, but Bukalapak already had it for some time when I joined them in August 2013). Our escrow service requires us to refund the funds to the buyers for transactions that are canceled by the sellers or the items are returned by the buyers, and we must forward the funds to the sellers for transactions that are successfully completed.

Because back in the early 2010s our banks hadn't yet provided good banking infrastructure for e-commerce systems with payment API integration and such, initially we needed to do fund transfers manually. And as we scale, we needed to use various workarounds to automate fund transfers for the major banks we supported. This is because we were forwarding the funds directly to the bank accounts configured by the sellers and buyers for their fund transfers immediately after each transaction.

At one point, we decided to just pool the funds in our system (under a feature called BukaDompet) until the users specifically requested the funds to be transferred to their configured bank accounts. This decision was made for two reasons:

- Reducing the number of fund transfers we need to perform daily, by allowing the funds from multiple completed transactions to be transferred to the users' bank accounts in batches by the users' demand instead of directly transferring them after each transaction. This makes it more efficient for our system to handle the load of transferring the funds to the users' bank accounts.
- Allowing the funds to be pooled in the users' Bukalapak accounts should encourage the users to shop more on the site as they could just use their BukaDompet balance to pay for transactions in Bukalapak instead of having to do more manual bank transfers when paying for the transaction (bank transfer was the most prominent payment method used by Indonesians when shopping online).

This sounds good as it reduced our operational workload for doing fund transfers to the users' bank accounts and pushed the users to spend more on our e-commerce site. But here comes the problem: it gave an incentive for cybercriminals to try and hijack our users' Bukalapak accounts, and then drain the funds the users had in their BukaDompet wallet.

So we started having to deal with financially-motivated phishing attacks targeting our users.

There were solutions such as two-factor authentication that we implemented at the time. But due to various reasons, the effectiveness wasn't really where we wanted it to be. We probably could make it more effective if we made it mandatory, but back then we were still aiming for high growth on various fronts so we were quite reluctant to push it since it might reduce the user and transaction conversion.

Nowadays most e-commerce sites operating in Indonesia are alright with making things a bit inconvenient for users in order to make their systems more secure, but back then it wasn't really the case.

In the context of this post, I'll discuss how I came up with a system that could automatically take down most phishing sites targeting our users within one hour after the phishing site's initial deployment.

# How the Problem and Solution Evolved

The anti-fraud operations team was the first people to notice and deal with the phishing problem. They started getting reports from users that their accounts were compromised, and they started dealing with it. After noticing that there were more cases than they could anticipate without support from the engineering team, the lead of the anti-fraud team went to my desk to tell me about the problem they were dealing with.

We had no idea how massive the phishing operations targeting our users were at the time. But based on the cases reported to the anti-fraud team by the victims, I could figure out a few patterns on how the phishing sites were set up.

- The phishing sites tend to use variations of the string "bukalapak" on their URLs.
- After the users enter their login credentials on the phishing sites, the sites will redirect the users to the real Bukalapak site.

Based on this information, I set up a system to keep track what sites that have suspicious variations of the "bukalapak" string had redirected users to our site, and set up a dashboard for the anti-fraud operations team to monitor if they encounter anything they considered phishing sites.

To make the life of the phishers harder, I also set up a few scripts I could run from my work computer to spam fake username/password pairs to fill the phishing site's DB with fake data and waste the attacker's time.

I analyzed a few of the phishing sites the anti-fraud team found from user reports, and I noticed a pattern that most of the phishing sites we found were storing the credentials of their phished victims in a text file that can be accessed as long as we know the text file's path in the sites. So we could retrieve the list of phishing victims to notify them and help them secure their online accounts.

Some of the later versions of the phishing site template seemed to have changed the location of the text file since I encountered a few exact same phishing sites but with the credentials written to a text file stored under a different name. But after I figured out those also, the phishers started to make things even harder as they started to get better in hiding those text files so we couldn't recover the list of victims from the later phishing sites.

The sites were hosted using various website hosting providers that provided free hosting packages, so I taught the anti-fraud operations team (they weren't really tech people, but could be quite savvy) how to figure out which hosting provider was used by a certain phishing site and how to find the email contact for reporting phishing sites hosted on their infrastructure. The hosting providers would take down the sites after we reported it, but some providers seemed to be ignoring our messages. Luckily, most of the phishing sites were hosted using the more responsive providers.

After a while, I noticed that more than 90% of the phishing sites we detected were hosted on only three or four hosting providers (and all of them are pretty responsive). So I set up our system to do a pattern matching on the URLs of the phishing sites that were redirecting their victims to our sites to identify which hosting provider was used, and had our system automatically send an email to those providers asking for the sites to be taken down.

This actually worked very well, since a lot of the phishing sites were taken down very quickly after it was detected to have redirected traffic to our system. Some of them seemed to be taken down from the traffic of the phishers getting redirected to our site when testing the phishing sites before sending it to real victims.

But having their phishing sites taken down too quickly seemed to have alerted the phishers that we had something that could take down their phishing sites very quickly, and they seemed to have experimented with a few setups to avoid their sites getting detected and taken down. It ended up with them deploying phishing sites that didn't redirect traffic to our site after the victims submitted their credentials to the malicious sites, and the anti-fraud team started getting reports of phishing sites that weren't detected by our phishing site detector.

So here I made the final modification to the anti-phishing system (the final modification I did before I left the company, at least). After analyzing all the reported phishing sites, I found that the vast majority of them were copying and pasting our HTML code directly, referencing the same JavaScript, CSS, and image assets URLs used on our real site. There were a few phishing sites that didn't do such things, but these sites visually didn't resemble our site's login page at all so fewer users were fooled.

With that information, I added a backend API endpoint whose URL was disguised as a JavaScript asset URL. In our front-end JavaScript code, I slipped several lines of code that check if the JavaScript was run on a browser that was opening our real website by checking the domain of the opened site. If it was not our site, then it would trigger an API call to the disguised endpoint, sending us information about the site that ran the JavaScript code to be checked by our anti-phishing system. If it was considered malicious, the anti-phishing system would automatically send an email to the hosting provider where that site was hosted, requesting a takedown of the site.

Up until I left the company, it seemed to have solved most of our phishing problem. The final modification I made practically made our front-end JavaScript code a malware that automatically takes down phishing sites if it were to be reused by phishers to make their phishing sites look and feel authentic. At that point, most phishing sites could be detected by our system during its initial deployment as soon as the site was rendered by the phisher when making sure that it was already live, and then our system would send an email to the site's provider asking it to be taken down. Most of the detected phishing sites were taken down within 30-60 minutes.

# What Happened After I Left

I'd never heard about Bukalapak facing a phishing crisis at such a large scale after I left, so I simply assumed that the anti-phishing system I developed and set up was sufficient. Probably they needed to add minor adjustments here and there, especially on the phishing site detection logic and the module that sends phishing site takedown requests to hosing providers.

The Indonesian government issued a regulation that banned local e-commerce sites from providing e-wallet features such as BukaDompet some time after I left, so in the end the BukaDompet balance of the users was moved to DANA, an Indonesian e-wallet company that's quite close to Bukalapak since they are both part of the EMTEK Group's investment portfolio. This shifted the responsibility to manage and secure the wallet balance to DANA, so I think Bukalapak users should be at less risk of getting targeted by phishers after that.

# Conclusion

Would that solution be feasible for today's phishing attacks? It's a bit hard to tell since at Cermati I've never worked on projects where I should protect our end users from phishers, so I definitely have been missing contexts on what phishing attacks targeting a web-based application's end users would look like today.

I did see a few phishing attempts while working at Cermati, never a site targeting our end users, but they were built and executed very differently from what I saw at Bukalapak. So I don't think the tricks I used at Bukalapak will work in this case.

But from what I've seen in the wild, I think local cybercriminals have shifted to targeting the victims' phones instead of simply compromising their e-commerce or e-wallet accounts. I've seen the rise of reports from people who were sent APK files via WhatsApp by people impersonating a delivery guy or someone they know, trying to fool them that the APK files are some other kind of files.

I also helped a friend who might have gotten love scammed last year, where they were tricked by their supposed "lover" into installing a phony TikTok Shop app on their device in order to get some extra income. They were told to deposit money to a bank account owned by TikTok as instructed by some phony TikTok customer service staff, they were told that this allowed them to be a premium seller or something for TikTok Shop's newly opened Australia region and TikTok Shop would handle all the item stock, delivery, and some other stuff. Sounds too good to be true? Of course it is, since it's not real and that's now how being an e-commerce seller works.

They lost practically all their life savings from that scam, they initially contacted me to borrow some money but since I felt like what they described to me was plenty suspicious, I decided to investigate it for a bit and reverse-engineered the TikTok Shop app they were asked to install by their "lover." It had some pretty funny modules in it that weren't supposed to be in a real TikTok app, and they downloaded it from some random AWS S3 bucket instead of the Google Play Store.

While phishing is still a thing, I think for targeting end users, cybercriminals tend to use even more sophisticated tricks nowadays.

# References

[Bukalapak Ceases Most E-Commerce Operations, Shifts to Utility Payments Amid Stock Decline](https://jakartaglobe.id/business/bukalapak-ceases-most-ecommerce-operations-shifts-to-utility-payments-amid-stock-decline)
