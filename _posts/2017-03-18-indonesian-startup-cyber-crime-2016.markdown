---
title:  "Indonesian Startup Cyber Crime 2016"
date:   2017-03-17 00:00:00
description: A Notable E-Commerce Cyber Crime Case
---

### Intro

As the time of the writing, I've been a security engineer in Bukalapak for seven months. Before that, I was a software engineer and handled their application security stuff. Bukalapak is based in Indonesia, and currently one of the biggest e-commerce company in the country.

During the time I focused on the application security both as a software engineer and a security engineer, I work closely with the anti fraud department. The anti fraud department handles the cases of users who become victims of fraud taking place in Bukalapak.

Among the most common cases of fraud were the takeover of accounts holding BukaDompet balance. BukaDompet is Bukalapak's wallet system, that enables users to keep their fund in Bukalapak. This balance can be used to pay for transactions, or withdrawn to the users' bank accounts. It was one of the biggest things in 2016's Indonesian e-commerce fraud scene.

### Not Just Us

Bukalapak was not the only target of the cyber criminals at the time. Tokopedia, another big Indonesian e-commerce with wallet system, was also targeted. Even GO-JEK, which operates mostly as a transportation company, wasn't safe. A lot of people feared they'd lose their wallet balance, and some even afraid to conduct online transactions.

One of my neighbors, knowing I was a software engineer in Bukalapak, told me that she got her account hijacked. Her wallet balance was used to conduct a transaction, but the items were sent to her address. It wasn't a big amount, but she stopped keeping wallet balance in any Interned-based applications afterwards.

Indonesian e-commerce companies improved the security of their system quite significantly to defend their users against the attacks conducted by the gang of criminals. While this case caused a great sense of insecurity among many Indonesian e-commerce users, it had a positive impact for the industry.

Many Indonesian startups' only concern is growth. For growth and agility they would gladly sacrifice other aspects, including security. This case brings the security problems in their business flows to light, as the awareness of their users increased. The users demanded the companies to make their system more secure, and the companies did so.

### Kaskus and the Syndicate

At the beginning of May 2016, there was an uproar at Kaskus. Kaskus is an Indonesian discussion board started by a few Indonesian students who studied in the US, which gathers a lot of users during the 2000s. It is regarded as the biggest Indonesian online community.

The uproar was caused by a user posting personal information of an e-commerce cyber criminal, along with screenshots of his braggings on Facebook regarding his criminal activities. He claimed to be able to bypass the security measures implemented by both Bukalapak and Tokopedia to protect the users' wallet balance.

The thread's original poster was threatened by cyber criminals who gathered in the thread using fake accounts, and the Facebook account of the criminals whose information he posted were inactivated soon afterwards. After some time, the criminals whose accounts were inactivated made new Facebook accounts with their real names though.

We were not really surprised that he would be able to bypass the security measures we implemented, as the implementation needed to be rolled step by step in our mobile apps to keep backward compatibility with the older versions for a while. We figured that they were keeping an older version of our mobile apps to bypass the security system we implemented starting in the later version.

The holes were closed at the end of May 2016 when most of the apps' users had moved to the newer version, as we stopped the support for the insecure old apps versions' flow when accessing the wallet.

### Criminals, Victims, and Law Enforcement

During the time we were rolling out the implementation of the wallet security measures, we managed to track down several criminals who caused a significant loss to our users by analyzing our system's logs and the other data we have in our system. We named several middle schoolers and high schoolers as cyber criminals to be reported to the law enforcement.

It was a bit unfortunate that most of the victims weren't willing to report their cases to the law enforcement. The Indonesian law enforcement process regarding cyber crime is relatively slow and difficult, that most of the victims didn't consider it as something worth doing.

The criminals are mostly teenagers who were fascinated with the stories of hackers, in a sense of breaking into other systems. Looks like in their attempt to learn, they joined a community of carders and learned how to conduct Internet fraud there. They mostly use phishing as their preferred attacking method, and after they gathered a number of valid login credentials they'd sell it. The price of a breached account was half of the available wallet balance in that account.

### Conclusion

The security breach cases in 2016 was a turning point where Indonesian tech startups start to put more focus on their system's security. But even if the system is hardened, it's impossible to be fully impenetrable for a few reasons.

The companies need to provide good UX to grow, which means that it shouldn't be so secure that it's painful to use. While several companies like banks might be able to pull it off, a regular tech startup would lose their user base if they go that way.

The users need to be well-educated regarding information security, which is a bit difficult to guarantee. A company might be able to give proper information security awareness training to their employees, but it's pretty difficult to do with their users. Especially when building consumer-based products.

Indonesian law enforcement still doesn't take cyber crime cases very seriously, mostly due to the lack of understanding of the technology it runs on and the impact of the crime. When I visited Polri (Indonesian national police) cyber crime division's office together with people from Bukalapak's legal and antifraud department, it wasn't so bad. But a coworker who needs to make the report somewhere else found it really hard to explain the case to the police.

Social and cultural state of a society also contributes to the likelihood of the people turning into cyber criminals. Cyber criminals bloom in places where the people are quite educated to have decent technical skills, but don't have any opportunity to contribute to society with their skills. They also bloom in places where standing up to the authority is considered a virtue.