---
layout: post
title:  "Analyzing a phishing email"
date:   2019-09-23
description: "Recently, my wife and I had some work done by a contractor. We have been awaiting an invoice. My wife received a interesting email with a word document attachment, and I decided to try to take it apart to see what it was."
categories: malware analysis phishing malicious word doc docm document
---

Backstory
=========

Recently, my wife and I had some work done by a contractor. We have been awaiting an invoice. My wife received a interesting email with a word document attachment,
and I decided to try to take it apart to see what it was.
Below is a screenshot of the email my wife received.

<img src="https://werdinfosec.com/images/2019-09-23/image1.png" class="centered" />

Digging In
============

I was quite proud of my wife for immediately recognizing this as a phishing attack. It's easy to see how this email might be skimmed, and this fake invoice could be opened without much thought.
I've always been interested in how nefarious actors are able to gain access to computers remotely.
The thing that really stuck out to me here was that this email came from a contractor we hired to do some work on our property. Being the curious individual I am, I had my wife forward the email to me.
At this point, google still had not flagged this as a suspicious file. I downloaded the file, moved it to a sandbox, and began analyzing it.
The first thing I did was check to see if the email address had been found in any publicly posted dumps.
Lo and behold, this email address has been found in 13 dumps!

<img src="https://werdinfosec.com/images/2019-09-23/image2.png" class="centered" />  

Initial Discovery
=================

After discovering the president of this company has been exposed in several leaks, it's not hard to imagine that it is someone else sending these emails.
A Word document can be unzipped, which may shed some light onto the inner workings of the file.

<img src="https://werdinfosec.com/images/2019-09-23/image3.png" class="centered" />

Interesting- what is a visual basic binary doing here?

Passing the file to virus total shows that only 10 of the 60 anti-virus vendors detected this attachment as a malicious file.
There are a lot of big names that missed this.

<img src="https://werdinfosec.com/images/2019-09-23/image4.png" class="centered" />

Introducing oletools
====================

[Oletools] is a set of python scripts for analyzing malicious macros that may be included with a variety of file types.
Oletools could be useful in an enterprise environment to confirm whether or not files have malicious macros.
I ran olevba against this .docm file that my wife received.

*command: olevba3 file.docm*
---------------------------
<img src="https://werdinfosec.com/images/2019-09-23/image5.png" class="centered" />

Conveniently, the program shows that certain keywords in the macro are suspicious and explains why. Olevba also suggests some other commands that may be useful such as --deobf and --decode.

*command olevba3 file.docm --decode*
------------------------------------
<img src="https://werdinfosec.com/images/2019-09-23/image6.png" class="centered" />

Very interesting- the author of the macro seems to be trying to hide something through obfuscation. I'm not a proponent of the addage "I'm not doing anything wrong, so I have nothing to hide";
however, when it comes to macros, you can feel pretty sure that this is clearly trying to hide something malicious.

Conclusion
==========

With the reemergence of the [emotet botnet], phishing attacks like this aren't going anywhere. The email came from the compromised email address of someone with whom we had corespondance.
It appears it is sophisticated enough to generate an email that is almost believable, aside from a few grammatical errors, and it is using the Invoice tactic.
In an enterprise environment, this little Word Doc could probably do some real damage. Let this be a reminder to properly train all employees to watch out for these things.
It is important to audit your team and make sure that everyone understands the implications of such an attack.

[oletools]: https://github.com/decalage2/oletools/wiki/Install
[emotet botnet]: https://en.wikipedia.org/wiki/Emotet
