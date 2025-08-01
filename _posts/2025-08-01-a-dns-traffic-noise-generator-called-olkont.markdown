---
layout: post
title: "A DNS Traffic Noise Generator Called Olkont"
date: 2025-08-01 00:00:00
description: A story on how I wrote a DNS traffic noise generator
tags: Security Networks
---

Back in July 2022, Indonesia's Ministry of ICT, which was called Kominfo (now renamed Komdigi), [banned online services](https://www.cnnindonesia.com/teknologi/20220731071756-185-828301/kominfo-resmi-blokir-paypal-yahoo-dkk) such as PayPal, Yahoo, and some others from being accessed by Indonesian Internet users. This was because the companies behind these services didn't register themselves as an electronic system provider (PSE) in Kominfo's database.

The registration requires them to comply with demands from Kominfo regarding content censorship and user personal data sharing, which was considered as a violation of human rights by some providers who then tried to talk Kominfo into reconsidering their policies. Aside from the policy regarding PSE registration, Kominfo also planned to have [a national DNS server](https://www.whiteboardjournal.com/ideas/human-interest/pemerintah-telah-lakukan-pelanggaran-ham-serius-lewat-permenkominfo-pse-dan-penerapan-dns-nasional/) for Indonesian Internet traffic to go through when accessing online services, which will help them in censoring information in the Internet.

These issues, of course, were met with a backlash from the Indonesian Internet community.

![Kominfo Backlash Tweet](/images/posts/kominfo-backlash-tweet.png)

Indonesian Internet users were tweeting "Kominfo kontol" (where "kontol" is a very obscene Indonesian curse word), dumping bottles of urine at the Kominfo office building, defacing government sites, going on protests, and leaking government systems' DB contents as a response to Kominfo's actions.

I myself was relatively unaffected by Kominfo's traffic censorship at the time, since I've been using [DNSCrypt](https://www.dnscrypt.org/) since 2016 or 2017 and also configured my DNS config to use DNS servers not managed by Indonesian entities, using [DNS over HTTP](https://datatracker.ietf.org/doc/html/rfc8484) or [DNS over TLS](https://datatracker.ietf.org/doc/html/rfc7858).

But still, I was concerned about the Internet freedom of the time, and considering that Kominfo seemed to be looking at [China's Great Firewall](https://en.wikipedia.org/wiki/Great_Firewall) as their inspiration. So then I read [this paper](https://arxiv.org/abs/2106.02167) by Hoang et al., which explains a series of experiments they did to figure out how the Great Firewall of China works.

I then thought: What would be the best move of an Indonesian citizen in the case where the national DNS server was put in place, and the government wouldn't listen to the people's aspirations against the censorship attempts?

For the more technically savvy Internet users, such as myself, we can always configure our DNS settings to avoid having the government intercept and manipulate the traffic. But not everyone's technically savvy enough to do that. Also, when everybody uses the same trick, it'd be easier for Kominfo to counter it later because of how homogenous the methods are.

So I thought, if the more technically savvy people can simply add random noises to their regular DNS query traffic to poison the national DNS server's DNS query logs so their DNS traffic analytics functionalities can't work as well as it should be when identifying the critical traffic that they need to censor based on the access volume and making the computation more intensive by adding more cardinality in their processed data, it will probably help the less technically savvy people in the process.

Then I decided to write a proof-of-concept of the DNS query log noise generator, called [Olkont](https://github.com/sdsdkkk/olkont) (given the tweets Indonesian Internet users were tweeting about Kominfo at that time, I'm sure you know how I came up with the name). Olkont is written in Python, with the functionality to load a list of domain names to query as noise and a list of DNS servers (along with their supported transport protocols). Olkont will then run on a loop, and on each iteration, it will sample domain names and DNS servers to be queried with the sampled domain names to generate traffic and log noise on the targeted DNS servers.

I had actually forgotten about Olkont. But earlier today, a coworker shared a GitHub repository with an inappropriate Javanese word as its name. Then I remembered that I once made a repository called Olkont, which is a play on a very inappropriate Indonesian word (which is also a Javanese word), because at that time, a lot of people were tweeting that word every day.

# References

[Kominfo Resmi Blokir Paypal, Yahoo Dkk](https://www.cnnindonesia.com/teknologi/20220731071756-185-828301/kominfo-resmi-blokir-paypal-yahoo-dkk)

[Pemerintah Telah Lakukan Pelanggaran HAM Serius Lewat Permenkominfo PSE dan Penerapan DNS Nasional](https://www.whiteboardjournal.com/ideas/human-interest/pemerintah-telah-lakukan-pelanggaran-ham-serius-lewat-permenkominfo-pse-dan-penerapan-dns-nasional/)

[DNSCrypt](https://www.dnscrypt.org/)

[DNS Queries over HTTPS (DoH)](https://datatracker.ietf.org/doc/html/rfc8484)

[Specification for DNS over Transport Layer Security (TLS)](https://datatracker.ietf.org/doc/html/rfc7858)

[Great Firewall](https://en.wikipedia.org/wiki/Great_Firewall)

[How Great is the Great Firewall? Measuring China's DNS Censorship](https://arxiv.org/abs/2106.02167)

[sdsdkkk/olkont: DNS query noise maker](https://github.com/sdsdkkk/olkont)
