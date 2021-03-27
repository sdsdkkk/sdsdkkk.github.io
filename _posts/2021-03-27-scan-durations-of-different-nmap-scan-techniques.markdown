---
layout: post
title: "Scan Durations of Different Nmap Scan Techniques"
date: 2021-03-27 00:00:00
description: Looking at the scan durations of various Nmap scan techniques
tags: Statistics Security
---

[Nmap](https://nmap.org/) is a free and open source network scanning tools commonly used for network security checking. Nmap provides several options we can use to tweak our network scanning configurations in order to be stealthier or to bypass network firewall rules.

In this experiment, we're using Nmap version 7.91 running on Kali Linux [WSL2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions) environment under Windows 10. From this experiment, I'm aiming to understand how much impact different scan technique options are having to Nmap's scanning speed.

# Experiment Plan

The following are the options Nmap 7.91 has regarding scan techniques.

```
$ nmap --help
Nmap 7.91 ( https://nmap.org )
...
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
...
```

In this experiment's setup we're going to use only one scanner host and one scan target, and we're also using only the default scan techniques without any further customizations. Therefore, we're eliminating the `--scanflags`, `-sI`, and `-b` scan technique options from our scenario. The official guide from Nmap on these scan techniques is available [here](https://nmap.org/book/man-port-scanning-techniques.html).

We're also excluding the UDP scan flag `-sU` from this experiment, as according to the official guide this scanning technique is extremely slow and is estimated to take more than 18 hours to scan all 65,536 ports. Since our scan is scanning the [default](https://nmap.org/book/performance-port-selection.html) 1,000 ports scanned by Nmap, we estimate it would take at least 30 minutes per scan run with the `-sU` flag active. Since we're doing 100 scans per configuration in the experiment, we're going to need at least about 300 minutes or 50 hours of scanning in order to collect the data for the UDP scan runtime, which is not something I want to do for the time being.

That gives us the flags we're going to test as follows:

- no flag
- `-sS`
- `-sT`
- `-sA`
- `-sW`
- `-sM`
- `-sN`
- `-sF`
- `-sX`
- `-sY`
- `-sZ`
- `-sO`

To measure the execution time, I'm referring to this line of the Nmap output and extract the scan time using `awk`.

```
Nmap done: 1 IP address (1 host up) scanned in 2.26 seconds
```

The time in the output format will always be given in seconds as defined in [this source file](https://github.com/nmap/nmap/blob/e2f1df924c096ba987c806303fe03455889349f4/output.cc). Therefore, we can reliably utilize `awk` to extract the time from the scan summary output string.

The scan runtime length data collection script is as follows.

```bash
#!/bin/bash

declare -a NMAP_FLAGS=('' '-sS' '-sT' '-sA' '-sW' '-sM' '-sN' '-sF' '-sX' '-sY' '-sZ' '-sO')
TARGET_HOST=192.168.0.1

for flag in "${NMAP_FLAGS[@]}"
do
  for i in {1..100}
  do
    echo "Loop no $i, flag: $flag"
    sudo nmap $flag $TARGET_HOST | grep "Nmap done" | awk '{print $11}' >> nmap-result$flag.txt
  done
done
```

# Experiment Result

The scan duration data is available on [this gist](https://gist.github.com/sdsdkkk/4e1737e4a5687f2a825841d4111cbe66) as a CSV file. The following figure plots the distribution of the scan duration data points from the experiment.

![Nmap Scan Duration Boxplot](/images/posts/nmap-scan-duration.png)

As we can see in the figure, the vanilla scan with no flag, ACK scan (`-sA` flag), SYN scan (`-sS` flag), and TCP Window scan (`-sW` flag) are the most consistently fastest scans in the experiment. The TCP connect scan (`-sT` flag) is also generally fast, but with more noticeably variations towards the slower end of the duration data points in the experiment.

Following the scans we've already mentioned are the SCTP COOKIE ECHO scan (`-sZ` flag), the SCTP INIT scan (`-sY` flag), and the IP protocol scan (`-sO` flag). The slower scans are the TCP Maimon scan (`-sM` flag), the TCP FIN scan (`-sF` flag), the TCP NULL scan (`-sN` flag), and the Xmas scan (`-sX` flag).

According to [the documentation](https://nmap.org/book/man-port-scanning-techniques.html) Nmap performs the TCP SYN scan (`-sS` flag) by default whenever possible when no scan technique flag is passed, and falls back to the TCP connect scan (`-sT` flag) when the TCP SYN scan can't be performed. This explains the similarity between the distributions of scan duration time between the no flag run and the `-sS` flag run in the experiment, since the experiment is performed under the condition where Nmap could use TCP SYN scan by default.

The TCP ACK scan (`-sA` flag) is quite similar with the TCP SYN scan in terms of the mechanism of the scan technique, but it only identifies whether a port is unfiltered without being able to tell whether the port is open or not. The TCP Window scan (`-sW` flag) is also similar to the TCP ACK scan in terms of mechanism, but with an extra advantage that it can exploit the implementation details of certain systems to determine whether a scanned port is open or closed. This explains the similarity of both scans' time duration distribution to TCP SYN scan.

Unlike the previous scans we've discussed, the TCP connect scan (`-sT` flag) performs a full proper TCP connection initialization sequence to the target. This makes the TCP connect scan to be somewhat slower to execute.

`-sY` and `-sZ` flags configure Nmap to perform SCTP INIT scan and SCTP COOKIE ECHO scan, respectively. [SCTP](https://www.rfc-editor.org/rfc/rfc4960.txt) is an alternative to TCP designed specifically to transport [PSTN](https://en.wikipedia.org/wiki/Public_switched_telephone_network) signaling messages over IP networks. Both scans work on a different protocol than the previous scans (which is done using TCP), which explains why their scan duration data distributions have considerably different ranges and median values compared to the previous TCP-based scans.

The IP protocol scan (`-sO` flag) tries to determine which [IP protocols](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers) are supported by target machines instead of open ports. The processes performed by this scan is somewhat different if compared to the previous TCP and SCTP scans, and we can see that the distribution range of the duration for this scan is quite different from the others.

The TCP NULL scan (`-sN` flag), TCP FIN scan (`-sF` flag), TCP Xmas scan (`-sX` flag), and TCP Maimon scan (`-sM` flag) are the slowest scans in this experiment. According to the [Nmap documentation](https://nmap.org/book/man-port-scanning-techniques.html) all four scans employ similar mechanisms, which differ from the other TCP-based scans we've previously discussed.

# Conclusion

From this experiment we found that using no flag when running the Nmap scan will default to the TCP SYN scan given that the TCP SYN scan can be performed. Since the conditions were met for the Nmap scan to default to the TCP SYN scan instead of the TCP connect scan, based on the scan duration data we've collected and compared, we can say that the TCP SYN scan was indeed performed instead of the TCP connect scan during the experiment.

As explained earlier in this article, we exclude the UDP scan (`-sU` flag) due to the UDP scan taking too long to finish, which is estimated to be at least around 30 minutes per scan run. In order to follow the same experiment procedures as the other scans, we need to run it 100 times and it would require at least about 50 hours of scanning in order for us to collect the data.

For the other scans we ran, based on the data we've collected we can group them into the following categories.

| Category           | Flags                      | Notes                                           |
|--------------------|----------------------------|-------------------------------------------------|
| Lightweight TCP    | `-sA`, `-sS`, `-sW`        | TCP, no full handshake, fast                    |
| Proper TCP connect | `-sT`                      | TCP, actually establish connection, quite fast  |
| Weird TCP          | `-sF`, `-sM`, `-sN`, `-sX` | TCP, weird bits are activated, slow             |
| IP                 | `-sO`                      | Scanning IP protocols instead of ports          |
| SCTP               | `-sY`, `-sZ`               | Scanning for SCTP ports instead of TCP/UDP      |

# References

[Nmap: the Network Mapper - Free Security Scanner](https://nmap.org/)

[Comparing WSL 1 and WSL 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)

[Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html)

[Port Selection Data and Strategies](https://nmap.org/book/performance-port-selection.html)

[nmap/output.cc](https://github.com/nmap/nmap/blob/e2f1df924c096ba987c806303fe03455889349f4/output.cc)

[sdsdkkk/nmap-scan-duration.csv](https://gist.github.com/sdsdkkk/4e1737e4a5687f2a825841d4111cbe66)

[Stream Control Transmission Protocol](https://www.rfc-editor.org/rfc/rfc4960.txt)

[Public switched telephone network](https://en.wikipedia.org/wiki/Public_switched_telephone_network)

[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)
