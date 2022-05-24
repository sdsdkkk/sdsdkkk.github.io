---
layout: post
title: "TP-Link Wireless Router ARP List Monitoring"
date: 2022-05-24 00:00:00
description: Keeping track of a TL-WR840N device's ARP list changes
tags: Security Networks
---

I got myself a [Raspberry Pi 3 model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) to tinker with in December 2022, and aside from doing some hobby machine learning projects I've also used it for [monitoring the downtime of my ISP](/2021-12-19/isp-uptime-checker-util). The [systemd](http://systemd.io/) config I made for this project is exactly the same as what I did for that project, so I'm skipping it here.

My network lacked the monitoring system to log when a device is connected to the network and when it is disconnected. So I checked around the router admin panel of my home router, which is a [TP-Link TL-WR840N](https://www.amazon.com/TP-Link-TL-WR840N-300Mbps-Wireless-Router/dp/B00ENM3RUQ/) device. So I poked around the web administrative dashboard to check the requests it's performing to retrieve the ARP list.

# The Interface

I wrote a Python module to be used as the interface to communicate with the router's back-end API [here](https://github.com/sdsdkkk/TPLinkARPMon). Below is the copy of it.

<script src="https://gist.github.com/sdsdkkk/ebc26f4b44012dfb8cee6b806f012e1e.js"></script>

To fetch the ARP information, first we need to perform a HTTP GET request to `http://<router-address>/cgi?5`. The request must be set with the following header.

```
Cookie: Authorization=Basic <base-64-of-admin-username-and-password-string-concatenation>
Referer: http://<router-address>/mainFrame.htm
```

The `Referer` header check is required by the router to validate that the back-end API is hit by the admin dashboard. It will not give us the ARP list otherwise.

The request must be set with a request body containing this magic string.

```
[ARP_ENTRY#0,0,0,0,0,0#0,0,0,0,0,0]0,0\r\n
```

Otherwise, we will not be getting any ARP list information from the back-end.

The API response is something like this.

```
[1,0,0,0,0,0]0
flag=0
ip=3232236630
mac=XX:XX:XX:XX:XX:XX
[2,0,0,0,0,0]0
flag=0
ip=3232236631
mac=XX:XX:XX:XX:XX:XY
[3,0,0,0,0,0]0
flag=0
ip=3232236632
mac=XX:XX:XX:XX:XX:XZ
[error]0
```

The value from the IP field needs to be converted to a hexadecimal representation first.

```
IP (int): 3232236630
IP (hex): 0xc0a80456
```

The resulting hexadecimal value needs to be processed partially as follows.

```
c0 (hex) = 192 (int)
a8 (hex) = 168 (int)
04 (hex) =   4 (int)
56 (hex) =  86 (int)
```

Hence, the IP field value of 3232236630 is equal to the IP address 192.168.4.86.

# The Main Process

The following is the script I wrote to create the long-running daemon process.

<script src="https://gist.github.com/sdsdkkk/c9c2e5c50cfc15c12b377537141b9e65.js"></script>

The fact that I hardcoded the router IP address, admin username, and password in the code is actually a bad thing from the security perspective. But since this is a one-off script that I wrote and run on a Raspberry Pi located in a very small network and accessible only to me, I'd take the chances.

This process will log the number of devices connected to the network along with the MAC addresses of the devices joining and leaving the network over time. The stdout stream will be redirected to write to a log file in the Raspberry Pi (configured from the systemd config).

The log data will be usable for various analytics purposes, such as detecting when one of my family members is leaving home and coming back home based on when their cell phone is leaving and rejoining the network. But for the time being, I don't have much data yet because this system has just been deployed for a few days.

# Conclusion

This project is meant to exploit more of my Raspberry Pi's capabilities, as it's mostly just idling around whenever I'm not using it for my personal data analytics and machine learning projects (which is most of the time). It does monitor my ISP's uptime status, but during the time my ISP is relatively stable there's not much value I can derive from it. Therefore, I try to add more work for it to do which also contributes to the security of our home network, as now I have the ability to audit the network to see if there's anyone other than our family members using it.

The router itself supports MAC address filtering to allow only a set of devices with registered MAC address to connect to the network. But it might be a bit inconvenient for the family, and the approach I'm currently taking allows me to track the time when they're present at home also, which gives me more understanding about what's happening at home to some extent.

# References

[Buy a Raspberry Pi 3 Model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)

[ISP Uptime Checker Util](/2021-12-19/isp-uptime-checker-util)

[systemd](http://systemd.io/)

[TP-Link TL-WR840N 300Mbps Wireless N Router](https://www.amazon.com/TP-Link-TL-WR840N-300Mbps-Wireless-Router/dp/B00ENM3RUQ/)

[sdsdkkk/TPLinkARPMon](https://github.com/sdsdkkk/TPLinkARPMon)
