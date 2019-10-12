## Welcome to myWriteup!

Hi there!

Brief intro about me; 
I am just a newbie in Infosec Industry, Capture-the-flag and also, in doing boxes on Hackthebox. 
I want to focus on DFIR but also, wants to learn some pentesting approaches, methodologies and techniques so Hackthebox is a great help.

Actually this is my first writeup of all the three (3) machines i have owned. 

I hope you will enjoy this and also, please correct me if there are some things that i have misunderstood about the machine and process or please do feel free to reach me out and to help me improve my understanding about the concepts because it will help me a lot. Thanks! 

### HTB-Writeup

Maybe if you are also a newbie in infosec/pentesting field like me, you may not heard yet about [**Hackthebox.eu**](https://www.hackthebox.eu).
Well, **Hack The Box** is simply just an online platform allowing you to test and advance your skills in cyber security. Well, just click the link, hack your way in and start the fun.


![HTB-Writeup Infocard](images/infocard.PNG)


### Recon
Initially, i run nmap to scan the machine and to look for those open ports and services that usually can be used for exploitation.
Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, whatv operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

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
And then i saw those open ports 22 and 80 (which is for 22 - SSH and 80 - HTTP). Usually, after doing nmap i am trying to look for the vulnerabilities of the open services and applications the machine are using

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/b3y0nd1xx7/HTB-Writeup/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.dddd

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
