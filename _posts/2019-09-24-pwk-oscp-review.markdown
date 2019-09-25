---
layout: post
title:  "How I passed the OSCP Exam on my first try"
description: "A review of the PWK course and the OSCP exam. This is an overview of the steps I took to pass the exam on my first try."
date:   2019-09-24 
categories: OSCP PWK Offensive Security Review pass penetration testing with kali linux hacking 
---

Introduction
============

There are a ton of OSCP guides and reviews. I decided to share my experience and review the Penetration Testing With Kali (PWK) course and the 
Offensive Security Certified Professional (OSCP) exam. I will try to provide my mindset and background experience, as well as share resources and exercises that I found helpful in my
journey to become OSCP certified. I was able to pass the exam, rooting all 5 boxes, on my first try due to careful planning and proper time management.

Background & Experience Level
=============================

I love linux, and I use it everyday. I have been interested in computer security for a long time. I had some experience using metasploit against metasploitable. 
I didn't really get my hands dirty hacking until I discovered [Hack The Box]. I understood networking concepts pretty well, and I knew how to use linux pretty well. 
I could read python but struggled to write complex scripts from scratch. I live rurally and haven't had the internet at my house for the last 5 years (I know, crazy).  

Planning
========

In late 2018, I started planning for how I was going to study for and take the exam. I knew that I wanted to start the labs sometime in the Summer of 2019. 
I decided that I would try to save some money by doing a lot of independent studying and only doing 30 days of lab time. 
I set aside Mondays, Wednesdays, and Fridays from 9-5, January through June, to study with [Hack The Box] and [VulnHub].
When July began fast approaching, I decided to push my lab date back until the beginning of August.

I completely understand that not everyone has 24 hours a week to study. I happened to have quite a bit of free time that allowed me to study in this manner. 
The absolute most important thing to do is make realistic goals and try to hold yourself to them.
The easiest way to ensure you stick to your plan is to tell multiple people what your schedule is going to be like. This is important, as it makes you accountable to the people you tell.
If you don't study, you will feel a little guilty. This really helped me to buckle down and study.

With my schedule all set up, I told my wife, my mom, my dad, and my friends what I was planning to do. I was excited and nervous.

Studying 1/3/19 - 8/8/19
========================

I made it my goal to try to hack every single box on this [list by TJNULL]. 
In order to do the retired machines on HTB, I had to purchase VIP; this cost me ~$12-15/mo. I think this is a pretty reasonable price. 
This list is really great practice for the PWK/OSCP. While I was going through this list, I attempted to do as much as possible without looking at any writeups. When I was completely stuck,
and after I got just about frustrated enough to throw in the towel, then and only then would I go and watch the [Ippsec] video. When I watched these videos, I would only watch just enough to get unstuck. 

I tried to keep really good notes as I was going, in an attempt to emulate the note taking I would need to do in the exam. I would review the notes after I completed each machine. 
I found it helpful to explain how each machine was exploited to my wife. 
By explaining the entire process out loud to another person, I was actually able to learn more about what I understood and what I did not.

One area in the syllabus that I was particularly worried about was the Buffer Overflow section. I tried to understand it by reading the Wikipedia page and everything else I could find
on the subject. In the end, this ended up being something I was very comfortable with. The point here is, by recognizing your weaknesses you have a targeted area to focus your energy.

As I was approaching the end of the list of HTB and VulnHub machines, I decided to purchase my lab time.

The Labs 8/9/19 - 9/9/19
========================

Upon receiving the confirmation of my purchase and lab date start time, I decided to take a short break from my thrice weekly studies. On the day my lab time started, I received an email 
containing the PWK pdf, the course videos, as well as my OS-ID username, password, and the vpn connection pack.

I decided to up my studying schedule to Monday through Saturday, 10-5. Knowing that I could receive an extra 5 points for completing the PWK pdf exercises and writing a professional report
on no less than 10 lab machines, I decided to strive for completing that. I spent the first 4 days going through the pdf and doing all of the exercises in order. I kept my notes in Cherry Tree.
There were several exercises I could not complete during the initial 4 days. Some of these exercises required actually exploiting machines in the lab.

After completing most of the exercises, I decided to start hacking my way through the lab network. The public subnet of the network contained 45 machines. Using some of the bash scripts
I had to make for the exercises, I was able to determine which hosts were live and resolve hostnames for most of the public network. I started at the top of the list and worked my way down. 
I found it extremely helpful to take many screenshots and try each exploit multiple times. I strived to understand each vulnerability as thoroughly as possible.

Nearing the end of my 30 days of lab time, I had rooted 28 machines in the public subnet and poked about a bit in two other subnets. 
At this point, I decided to finish up my exercise report and write a proper report on the 10 machines I most enjoyed exploiting. 
I also decided to schedule my exam for 9 a.m. the day after my lab time ended.

The Exam 9/10/19
================

The morning of the exam, I woke up early and got properly caffeinated. I also ate a hearty and healthy breakfast before taking a seat at my desk. 
The information about connecting to the proctoring software was straight-forward. The proctoring software recently changed to be browser based and worked fine with Google Chrome.
You have to connect to the proctor 15 minutes before the exam. You must share your webcam and screen(s) with them. Once everything is set up and working, they give you the go-ahead.

Upon connecting with the new vpn connection pack, you can access a control panel for machine reverts, submitting proof, and reading the point value and objectives for the 5 targets.
I can't say much about the types of machines, but there is a 25 point Buffer Overflow machine, another 25 point machine, two 20 point machines, and finally, one 10 pointer.

My strategy was to immediately start scans on the other 4 machines using an awesome tool called [AutoRecon]. 
At 9:00, as AutoRecon began discovery and enumeration of 4 of the machines, I began the buffer overflow.
I wanted to spend 1 hour on the Buffer Overflow machine so I could have as much time as possible for the rest of the exam. For the buffer overflow, you are provided with a debugging VM.
The debugging VM has the service to be exploited, a proof of concept, and a debugger. I had a couple issues with my connection during this portion of the exam, 
but by around 10:30 I had a working exploit and was able to gain a shell on the BOF exam machine. I submitted my proof.txt to the control panel and took my first break.

When you take breaks during the exam, you just simply type into the chat, the proctor responds, and you take your break. You can break for as long as you'd like. 
When you are ready to start again, you type into the chat and, once the proctor gives you the go ahead, you are free to resume.

I came back from my break and began to review the findings for the other 25 point box. I was able to get a shell in about 30 minutes. I then got stuck for a long time doing the privilege escalation.
I could see the path forward but couldn't quite get it right. I decided I had spent enough time and needed to move on. I took my lunch at 2:00

After returning from lunch, I was able to take down the two 20 point targets with no real issues. I then moved on to the dreaded 10 point box. 

I got so incredibly stuck on the 10 point box. I was enumerating every single service on the machine. I looked everything over at least 3 times. I struggled to find any path forward.
I was just about to start throwing every exploit I could find at this. Then, I saw it. It was one line I skipped over at least 20 times. 
I felt very dumb because after seeing this line I was able to root the box in 10 minutes. I guess this is another lesson. 
If you are really stuck, either move on, or be sure you've actually looked at everything.

Knowing that I already had 75 points plus half-ish credit for the 25 point machine I got stuck on, I almost ended right here. I did want the bragging rights of rooting every machine though.
This is a little bit difficult to describe without revealing any information I am not allowed to share, but basically, I thought about the privilege escalation again and it just clicked.
I regained my shell and did the priviliege escalation, and that was it.

The very last thing I did was double check my work. I wanted to make absolutely sure that I had every screenshot that I would need to do the report the following day.
I also double checked that I had submitted all of the proof.txt and local.txt required. I was very happy I did this because had I not, I wouldn't have received credit for one of the machines.
Additionally, I discovered a few screenshots I needed to show full proof for the buffer overflow machine.

Satisfied I had everything I needed, I decided to inform the proctor that I was done with the exam and breathed a sigh of relief. 
I went to the kitchen, got a glass of wine, and couldn't stop smiling.

The Report 9/11/19
==================

When I woke up, after my usual routines, I got to work on my report. I found that, while it was surprisingly simple, I was extremely nervous. I was worried I would miss something.
I feared I might say something incorrect. After about 5 hours of working on my report, I archived it, along with my lab report and exercises, and sent it off to OffSec.
I received a response a few hours later that they had received it. I was informed it would take 10 or less business days to receive my results.

Results 9/15/19
===============

When I woke up, I checked my email, as I had been doing every few hours since I sent off my report. I was sure I must have done something wrong.
As I scrolled through my emails, I noticed I had received Certification Exam Results. Anxiously, I opened the email and discovered I was now an OSCP!

<img src="https://werdinfosec.com/images/2019-09-24/image1.png" class="centered" />

Conclusion
==========

I thoroughly enjoyed the course and the exam. I really have an appreciation for the time it must have taken to put together the lab and exam network.
One thing I was very glad to hear from people who took the exam before me was to try privilege escalation without kernel exploits wherever possible.
As the lab network is aging, more and more of the machines may have unintended vulnerabilities.
One area where I felt the course materials were lacking was privilege escalation. PE can be completed in a plethora of ways and, as such, can be difficult to teach.
There are, however, in my opinion, too few examples in the pdf.

I feel that the lab targets being slightly dated really doesn't matter much. This course's intention is to help you create a methodology for testing targets.
The methodology you form through this exam can be applied to new or old systems. This exam also proves that you are able to write a professional penetration testing report.

I would say to anyone interested in this course that you should definitely know a little about hacking before going into it, but for the most part, you will learn everything you
are required to know to complete the exam. I am very proud to call myself an OSCP because I worked hard to receive this certification.

If anyone would like to reach out to me, I can be found on the HTB and OSCP discord servers. Email is also accepted.

discord: werdhaihai
eemz: werd at werdinfosec.com

[Hack The Box]: https://hackthebox.eu/
[VulnHub]: https://vulnhub.com
[list by TJNULL]: https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=1839402159
[Ippsec]: https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA
[AutoRecon]: https://github.com/Tib3rius/AutoRecon
