---
layout: post
title:  "Analyzing a phishing email"
date:   2019-09-19 08:43:00
categories: malware analysis phishing malicious word doc docm document
---

Backstory
=========
Recently my wife and I had some work done by a contractor. We have been awaiting an invoice. My wife received a suspicious email with a word document attachment and i decided to try to take it apart and see what it was.
Below is a screenshot of the email my wife received.

<img src="/blog/images/2019-09-19/image1.png" class="centered" />

Some section title
==================

I was quite proud of my wife for immediately recognizing this as a phishing attack. It's easy to see how this email might be skimmed, and this fake invoice could be opened without much thought. 
I've always been quite interested in how malicious actors are able to gain access to computers remotely.
The thing that really stuck out to me here was that this email came from a contractor we hired to do some work on our property. Being the curious individual I am, I had my wife forward the email to me.
At this point google still had not flagged this as a malicious file. I downloaded the file, moved it to a sandbox and began analyzing it. 
The first thing I did was check to see if the email address had been found in any publicly posted dumps.
Low and behold this person's email address has been found in 13 dumps!

<img src="/blog/images/2019-09-19/image2.png" class="centered" />  

Some section title
==================

After discovering the president of this company has been exposed in several leaks it's not hard to imagine that it is someone else sending these emails. 
A Word document can be unzipped which may shed some light onto the inner workings of the file.

<img src="/blog/images/2019-09-19/image3.png" class="centered" />

Interesting what is a visual basic binary doing here.

Passing the file to virus total shows that only 10 of the 60 anti-virus vendors detected this attachment as a malicious file.

<img src="/blog/images/2019-09-19/image4.png" class="centered" />

[oletools]: https://github.com/decalage2/oletools/wiki/Install
