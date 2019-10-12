## Welcome to myWriteup!

Hi there!

Brief intro about me; 
I am just a newbie in Infosec Industry, Capture-the-flag and also, in doing boxes on Hackthebox. 
I want to focus on DFIR but i also want to learn some pentesting approaches, methodologies and techniques so Hackthebox is a great help.

Actually this is my first writeup of all the three (3) machines i have owned. 

I hope you will enjoy this and also, please correct me if there are some things that i have misunderstood about the machine and process or please do feel free to reach me out and to help me improve my understanding about the concepts because it will help me a lot. Thanks! 

### HTB-Writeup

![HTB-Writeup Infocard](images/infocard.PNG)

Maybe if you are also a newbie in infosec/pentesting field like me, you may not heard yet about [**Hackthebox.eu**](https://www.hackthebox.eu).
Well, **Hack The Box** is simply just an online platform allowing you to test and advance your skills in cyber security. Just click the link, hack your way in and start the fun.

So, let's start!

### Recon
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
After the scan, i saw those open ports 22 and 80 (which is for **(22)SSH** and **(80)HTTP**). 
Usually, after doing nmap i am trying to look for the vulnerabilities of the open services and applications the machine are using and if there are available exploits that can be used to take advantages of that vulnerability.

I've tried to search for the vulnerabilities of the **OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)** and **Apache httpd 2.4.25** but there is none much related.

Then that's the time i've accessed the port 80 and saw this website.

![HTB-Writeup Website1](images/website1.PNG)

At first, there is a rabbit hole in the website stating about the **Eyeore DOS Protection Script** but it is not related to the machine. Then i went back again to review my nmap scan and nocticed the **robots.txt**

 A **robots.txt** file tells search engine crawlers which pages or files the crawler can or can't request from your site.
 
 ```markdown
 80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/writeup/
 ```
 When i accessed the robots.txt this is the output
 ![HTB-Writeup robots.txt](images/robots.PNG)

 Of course we are trying to hack our way in to own this machine so, i still try to access the **/writeup** directory and saw this unfinished website of the owner.
 
 ![HTB-Writeup /writeup](images/writeup.PNG)

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/b3y0nd1xx7/HTB-Writeup/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.dddd

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
