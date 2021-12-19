---
layout: post
title: "ISP Uptime Checker Util"
date: 2021-12-19 00:00:00
description: Implementing an ISP uptime checking system util
tags: ComputerScience SoftwareEngineering Networks
---

Recently I got myself a [Raspberry Pi 3 model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) to tinker with during the Christmas holiday. I'm planning to use it for a mini home server to host my experimental personal projects.

To get immediate value from the hardware, I decided to have a simple daemon for it to run in order to track the uptime of the ISP I'm using. The ISP is generally stable, but there are times when they are not very reliable. During their unreliable period, at best they can have a few minutes of outage every now and then and at worst they can have a full uninterrupted several days of downtime.

We can call the ISP to have the downtime period deducted from our monthly bills, but the amount of the deduction must be requested specifically (how many days or hours of downtime we experienced that we want to be deducted). Therefore, if we don't track the downtime meticulously, we might miss a few hours (or even days, if we don't remember it correctly when complaining) of downtime in the complaint report and keep paying for the downtime period. This is where this project comes into play.

# Problem Definition

Suppose we have a job $$J(H_t)$$ that performs connection checks periodically to publicly-available hosts, where $$H \in \{h_1, h_2, ..., h_n\}$$ is the status of a set of hosts when checked at time $$t$$.

If $$\exists~h \{h \in H \vert h = True\}$$ at time $$t$$, then $$J(H_t) = True$$. Else, if $$\forall~h \{h \in H \vert h = False\}$$ at time $$t$$, then $$J(H_t) = False$$.

If $$J(H_t) \neq J(H_{t - 1})$$, then the system undergoes a status change from up to down or vice versa and the system time must be logged for reporting before waiting for the next time step $$t + 1$$ before repeating the procedure. Else, if $$J(H_t) = J(H_{t - 1})$$, then the system should do nothing and wait until the next time step $$t + 1$$ before repeating the procedure.

# Technical Considerations

We're using the following hosts to be checked periodically as the set of host status $$H$$:

- `google.com`
- `facebook.com`
- `firstmedia.com`

Google and Facebook are included to the list due to their reputation for system uptime, and due to the fact that they both manage their own infrastructure which makes it quite unlikely for both to be down at the same time.

FirstMedia host is included to the list as a representative of a system owned by the ISP, which can be used as an indicator if the ISP network is still up in the case where both Google and Facebook is down, which may be caused by broken links toward Google and Facebook's hosts in the network.

The system is implemented as a long running process daemon which is managed by [systemd](http://systemd.io/) instead of a scheduled [cron](https://en.wikipedia.org/wiki/Cron) job. The reason is that it enables the job to keep running without interruption while keeping the state of the $$J(H_{t - 1})$$ in memory, whereas if we're using cron scheduled job we need to keep the state either in storage or somewhere else.

We're using Python 3 to implement the job, as it's readily available in the [Raspbian OS](https://www.raspbian.org/) installation used for the Raspberry Pi device.

I decided to use [ping3](https://pypi.org/project/ping3/) package for performing network connection checks using ICMP protocol instead of running `ping` system command in the implementation. Initially, I planned to do my own connectivity checker implementation using [Scapy](https://scapy.net/) and simply try to initiate a three-way handshake up to the SYN/ACK response in order to check if the host is reachable with minimum network packet overhead. But in the end I decided not to, because sending abnormal TCP package continuously to the hosts might be detected as a suspicious activity by their security system.

# Implementation

The following code is available on GitHub as [a gist](https://gist.github.com/sdsdkkk/b28a96f83c4c7c8a3b9075b5d8231e93).

<script src="https://gist.github.com/sdsdkkk/b28a96f83c4c7c8a3b9075b5d8231e93.js"></script>

We have a delay of 60 seconds before repeating the network uptime checking sequence in the main loop.

The following systemd service config file is available on GitHub as [a gist](https://gist.github.com/sdsdkkk/c33c263457016673a4716651283d5b43). In this setup the Python script is stored inside the user `automaton`'s home directory at `/home/automaton/network-utils/isp-uptime/uptime-check.py`.

<script src="https://gist.github.com/sdsdkkk/c33c263457016673a4716651283d5b43.js"></script>

The environment variable `PYTHONUNBUFFERED` must be set to `1` in order for the Python script output to be logged by systemd in real time. Otherwise, the output will be kept in the buffer and will not be logged in the target config file `/var/log/isp-uptime.log`.

[This article](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files) by Justin Ellingwood gives some explanation on the options for systemd unit file configuration.

# References

[Buy a Raspberry Pi 3 Model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)

[systemd](http://systemd.io/)

[cron](https://en.wikipedia.org/wiki/Cron)

[Raspbian](https://www.raspbian.org/)

[Scapy](https://scapy.net/)

[ISP uptime checker process script](https://gist.github.com/sdsdkkk/b28a96f83c4c7c8a3b9075b5d8231e93)

[ISP uptime systemd config file](https://gist.github.com/sdsdkkk/c33c263457016673a4716651283d5b43)

[Understanding Systemd Units and Unit Files](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)
