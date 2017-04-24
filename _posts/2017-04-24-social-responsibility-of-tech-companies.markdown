---
title:  "Social Responsibility of Tech Companies"
date:   2017-04-24 00:00:00
description: Beyond Performance Indicators
---

Tech is becoming even more mainstream, and everybody's dreaming of their own successful tech startups. In Indonesia alone, a few startups have gained their position and matured into the next stage. Still, with population exceeding 200 million people, Indonesia is a large market to own. Many of the maturing tech companies are still pushing their business growth.

President Joko Widodo's reign also tries to support the growth of tech startups, believing it's the economy of the future[1]. In 2016, the number of Internet users in Indonesia surpassed 132 million people, rose more than 50% compared to the number of Internet users in 2015[2]. That's without the rest of population joining the bandwagon yet.

It's understandable how growth is essential to the success of tech startups, especially those targeting general market. Being the market leader means long term survival and profitability of the company. But should growth be pursued at any cost?

Every decision by the companies could have lasting impacts to their users and the society in general. What is seen to bring a positive impact to the company right now, might affect other parties negatively in the long term.

The following are several things that might need to be considered as the companies pursue their growth target.

#### 1. Financial Impact to the Users

Seeing the phrase "financial impact", the first thing that popped to our minds might be the e-wallet features provided by many startups. Starting from Bukalapak's _BukaDompet_, Tokopedia's _Saldo Tokopedia_, to GO-JEK's _GO-PAY_. Even some local startups whose products are still in very early phase already planning to have the e-wallet feature in their milestones.

These e-wallet features are juicy targets for local cyber criminals. With improperly designed e-wallet features, they don't need to have advanced technical skills to drain their victims' e-wallet balance empty. It happened in 2016 with several Indonesian startups[3].

But the financial impact isn't limited only to e-wallet balance. Let's say we have a C2C marketplace e-commerce which prioritize products by reputed sellers compared to newly registered ones in the search engine and product recommendation system. The reputation is measured by the feedback given by their buyers after the transaction flow is completed.

By observing how the products are sorted and how the buyers are guided by the company to choose reputable sellers, some sellers might resort to fake transactions to gather positive feedbacks. After gathering enough positive feedbacks from fake transaction, the fraudulent sellers now have the upper hand in the system compared to the honest ones. Not to mention if the company issues discount vouchers subsidized by the company itself, they can also make a big buck by using the discount vouchers in their fake transactions while gathering the feedbacks.

After a while, these fraudulent sellers will start to get more legitimate buyers compared to their honest counterparts. By any chance they managed to grow so big that punishing them would hurt the company performance, but letting them run wild would hurt the honest sellers' sales.

What should the company do at this point? Depends on what the priorities of the top management, of course. Ideally, the company should protect the ecosystem so nobody could reap any reward for frauds they committed in the past even if they aren't committing frauds anymore. Otherwise, it would lead to other sellers walking the fraudulent paths and the honest ones struggling to survive.

#### 2. Stored User Information

How do the companies manage the information entrusted by their users? Who can access them?

These questions are sometimes (or often, I'm not very sure) not explicitly answered in the terms of use. Some companies are compliant to security standards enforced to protect their users' information. In the context of Indonesian startups, we can point out to Midtrans[4] and Tokopedia[5] for being compliant to PCI DSS to protect their users' credit card information.

Look at the security standards they enforced, and for which they're going to be audited periodically. We can pretty much guess how they store the information and who can access them by checking out the regulation for the standards.

Still, the standards only work regarding the information regulated in order to be compliant. There's no guarantee regarding the other information.

Let's be positive. Even if the company isn't compliant to any security standard, they're going to protect it as much as they're aware about the value of the information they hold. At least from outsiders, since some companies allows many of their employees have access to the user information for the sake of operational efficiency.

Not much can be done regarding this aside from properly regulating the access inside the company and securely building their software. At least the user need to be aware on where the information they submitted will go, and what are the company's terms regarding the information.

#### 3. Empowering Users

A company lives by its users (also by its employees and investors, of course). Therefore, the company will try to build their product so their users would use it again and again.

In designing the product, companies should consider how the users would be benefited from it and how the users might be constrained by it.

The companies are able to see the benefits and constraints given by a certain design in the product by conducting UX researches, and they tend to go for designs that gives the most benefit and the least constraint. Now, who defines what are the benefits and what are the constraints?

Benefits and constraints are defined by the company building the product, and both are aligning to the company's goals: maximizing certain aspects from users' interaction with the system. The effectiveness is measured by how long the users stay on a page, how much clicks are done, how big the conversion rate in the funnel, and so on. Ultimately, this will lead to achieve the company's goals.

As the company's trying to get the users to interact with their product as much as possible, it might play some psychological manipulations to keep them stuck with the product[6]. Depending on what the product is and what situation the users are in, sticking with the product might or might not give more benefit than constraint to the users.

Users developing addiction to a certain product might be a good news to the company. They can claim that their user retention rocks or whatever it is. Well, is it a good news to the users? If it's an e-commerce app where the users are sellers who keep getting purchase orders to the point that they couldn't leave the app it might be. Yet, if it's an e-commerce app where the users are sellers who keep getting manipulated to spend money on premium features without getting much purchase orders even after using the premium, maybe not so good.

The company's goals aren't the only one that need to be achieved. The design has to be beneficial to the users, without putting them at risk and giving them unnecessary addiction to the product.

#### 4. Securing the System

Designing and implementing the system in a secure way should be one of the key responsibility of the companies. A system that's secure by design and implementation should contribute to the first and second points in this post.

Even if the users' financial needs are protected with security features, it wouldn't be much help if the security features themselves have security flaws.

The same would apply to the user information management. Strict access policies wouldn't worth much if the system is insecurely designed and implemented. There would be holes that can be exploited to get around the access policies.

Aside from that, there are countless of possible attack vectors that can be carried out and might impact the users, the company itself, or other systems. Is it possible to perform any of the attacks through the product as the medium?

If the product is partially or fully web-based, it might be wise to take some time and check out common vulnerabilities according to OWASP[7] and understanding the risk posed by each of them. Of course those aren't the only possible flaw to be exploited in a web-based system. There might be a flaw unique to a company's app due to bad programming practices or badly designed features. And that's only the web app's security, there's still a lot more stuff to secure.

#### 5. Contributing to Development in the Field of Technology

Companies without a lot of resource might not have fancy internal projects to be open-sourced like Google has. Still, that shouldn't stop the companies from aiming to contribute. The contribution itself doesn't have to be so fancy, even simple libraries can be useful if shared to other people.

But in a country where the technology literacy isn't very high, and the quality of education isn't as good as many others, educating non-technical people and helping less-skilled engineers will make difference. Many university graduate in computer science or related fields around here haven't really master the basics when graduating from their respective universities.

People who're privileged enough to get better-quality education and experience to master the essential stuff might make only a small minority in the local technology industry. If some of them sincerely interested in teaching the less-skilled ones and managed to do it well, that should add more quality engineers in the field.

Some people doesn't have the skills simply because they have no idea where to go, due to different circumstances they're facing. The Pencilsword did a decent job describing how this difference affects people[8]. Given proper directions, access to resources, and opportunity, the ones willing to grow should be able to grow well.

The elitist ones getting these people, who happen to be less skilled at the present, to believe they're inferior beings that should know their place wouldn't help in this case. Who knows if something brilliant is laying dormant in their minds, simply because they has no idea how to realize that yet?

For the more experienced ones who need to improve, they'll also need have the humility to accept that some of their current practices might be wrong. Some of them already have their mindset set in stone. It would be really hard to change, and they might take it personally if someone asks them to consider other methods.

In the ideal world, open-mindedness and willingness to change for the better should be the main factors for someone to improve. While the reality is far from ideal, the companies could at least aim to be closer to the ideal one. Of course only if the companies have the same ideal view, they might have something different though.

It'd be nice if the companies could also consider the strengths and weaknesses of each individuals working with their technology and lead them properly. Sometimes, in the attempt to pursue their growth target, the company neglected the individuals and they end up with hampered growth.

### References

[1] [Jokowi "Pusing" Perkembangan "Startup" Indonesia Tertinggal dari Negara Lain](http://bisniskeuangan.kompas.com/read/2016/04/27/114500026/Jokowi.Pusing.Perkembangan.Startup.Indonesia.Tertinggal.dari.Negara.Lain)    
[2] [2016, Pengguna Internet di Indonesia Capai 132 Juta](http://tekno.kompas.com/read/2016/10/24/15064727/2016.pengguna.internet.di.indonesia.capai.132.juta.)    
[3] [Indonesian Startup Cyber Crime 2016](http://sdsdkkk.github.io/2017/indonesian-startup-cyber-crime-2016/)    
[4] [Aman dalam Berbisnis dan Membayar Online](https://midtrans.com/security)    
[5] [Bukalapak dan Tokopedia Jelaskan Aplikasinya yang Lacak Data Pengguna](http://tekno.kompas.com/read/2016/11/02/13020077/bukalapak.dan.tokopedia.jelaskan.aplikasinya.yang.lacak.data.pengguna)    
[6] [How Technology is Hijacking Your Mind — from a Magician and Google Design Ethicist](https://journal.thriveglobal.com/how-technology-hijacks-peoples-minds-from-a-magician-and-google-s-design-ethicist-56d62ef5edf3)    
[7] [Category:OWASP Top Ten Project](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)    
[8] [The Pencilsword: On a plate](http://thewireless.co.nz/articles/the-pencilsword-on-a-plate)