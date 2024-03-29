---
layout: single
title: "PWK and OSCP Review"
header:
  teaser: oth.png
  overlay_image: oscp/oscpcover.png
related: true
comments: true
---

A journey that lasted for a couple of years, OSCP has always been a goal when I started my infosec journey. Met some great people on this wonderful journey, 
who helped me greatly in improving my skills and in my personal growth. Finally, on September 28, 2020, I received the email which I have always dreamt about. 

It was an ecstatic moment, after completing 2 months of gruesome labs and the newly updated course had more exercises which added to this greatly. 
OSCP would be the toughest exam which I have given to date, After 24 hours of the Exam everything paid off finally. 


## preparation 

I would recommend everyone to complete the [__TJ Null's OSCP list__](https://docs.google.com/spreadsheets/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/edit#gid=0){:target="_blank"}"
before enrolling for the course. As multiple machines are quite similar to the OSCP labs.

One month before starting the lab time, I had completed most of the machines from TJ Null's List and focused more on buffer overflow and privilege escalation.

The Udemy courses and Tryhackme rooms go hand in hand, the official materials lack a bit on the privilege escalation side and these courses would help in 
overcoming those.

- [Windows Privilege Escalation for OSCP & Beyond! - Udemy Course](https://www.udemy.com/course/windows-privilege-escalation/){:target="_blank"}
  - [Windows Privilege Escalation - TryHackMe Room](https://tryhackme.com/room/windows10privesc){:target="_blank"}
- [Linux Privilege Escalation for OSCP & Beyond! - Udemy Course](https://www.udemy.com/course/linux-privilege-escalation/){:target="_blank"}
  - [Linux Privilege Escalation - TryHackMe Room](https://tryhackme.com/room/linuxprivesc){:target="_blank"}
- [dostackbufferoverflowgood](https://github.com/justinsteven/dostackbufferoverflowgood){:target="_blank"}
  - [Buffer Over Flow - TryHackMe Room](https://tryhackme.com/room/bufferoverflowprep){:target="_blank"}
- [Proving Grounds](https://www.offensive-security.com/labs/individual/){:target="_blank"}

All these are extra materials that would help in greatly speeding up the lab completion.

> Penetration Testing with Kali Linux is a foundational security course, but still requires students to have certain knowledge before attending the online training class. A solid understanding of TCP/IP, networking, and reasonable Linux skills are required. 
Familiarity with Bash scripting along with basic Perl or Python is considered a plus. This advanced penetration testing course is not for the faint of heart; 
it requires practice, testing, and the ability to want to learn in a manner that will grow your career in the information security field and overcome any learning plateau.


## Resources & Tools

Most of the resources and tools listed here are based on my personal experience during the labs and exams. There are many other resources but rather 
than the quantity, I have tried to focus on quality resources that helped me.

* Discord Group
  - [Infosec-prep](https://discord.com/invite/infosecprep){:target="_blank"}
* Enumeration
  - [Autorecon](https://github.com/Tib3rius/AutoRecon){:target="_blank"}
  - [nmapAutomator](https://github.com/21y4d/nmapAutomator){:target="_blank"}
  - [Common Ports](https://sushant747.gitbooks.io/total-oscp-guide/content/list_of_common_ports.html){:target="_blank"}
* Windows Privilege Escalation
  - [Windows Privilege Escalation - Checklist](https://github.com/netbiosX/Checklists/blob/master/Windows-Privilege-Escalation.md){:target="_blank"}
  - [Windows Privilege Escalation](https://securism.wordpress.com/oscp-notes-privilege-escalation-windows/){:target="_blank"}
  - [Windows Privilege Escalation - TryHackMe Room](https://tryhackme.com/room/windows10privesc){:target="_blank"}  -- This would help in greatly improving Privilege Escalation skills as it goes through all the available methods in much more depth.
  - Tools:
  	- [Complete Set of Windows PrivEsc Tools](https://github.com/Tib3rius/Windows-PrivEsc-Tools){:target="_blank"}
	- [WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS){:target="_blank"}
	- [Seatbelt](https://github.com/GhostPack/Seatbelt){:target="_blank"}
	- [wesng](https://github.com/bitsadmin/wesng){:target="_blank"}
	- [Juicy Potato](https://github.com/ohpe/juicy-potato){:target="_blank"}
* Linux Privilege Escalation
  - [Linux Privilege Escalation](https://payatu.com/guide-linux-privilege-escalation){:target="_blank"}
  - [SUID List](https://pentestlab.blog/2017/09/25/suid-executables/){:target="_blank"}
  - [GTFOBins](https://gtfobins.github.io/){:target="_blank"}
  - [Linux Privilege Escalation - TryHackMe Room](https://tryhackme.com/room/linuxprivesc){:target="_blank"} -- Highly Recommended for improving Privilege Escalation skills
  - Tools:
  	- [Complete Set of Linux PrivEsc Tools](https://github.com/Tib3rius/Linux-PrivEsc-Tools){:target="_blank"} 
	- [linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration){:target="_blank"}
	- [linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS){:target="_blank"}
* File Transfer Methods
  - [Windows File Transfer](https://isroot.nl/2018/07/09/post-exploitation-file-transfers-on-windows-the-manual-way/){:target="_blank"}
  - [Linux File Transfer](https://sushant747.gitbooks.io/total-oscp-guide/content/transfering_files.html){:target="_blank"}
* Buffer Over Flow
  - [dostackbufferoverflowgood](https://github.com/justinsteven/dostackbufferoverflowgood){:target="_blank"} -- Best resource to start BOF from scratch 
  - [Buffer Over Flow - Checklist](https://github.com/Arken2/Everything-OSCP/blob/master/Checklists/WindowsBufferOverflowChecklist.pdf){:target="_blank"}
  - [Buffer Over Flow - TryHackMe Room](https://tryhackme.com/room/bufferoverflowprep){:target="_blank"} -- Highly Recommended for identifying bad chars and for practice 
* Exam Report Template
  - [Exam + Lab report](https://github.com/whoisflynn/OSCP-Exam-Report-Template){:target="_blank"}
* Taking Notes
  - [CherryTree](https://github.com/giuspen/cherrytree){:target="_blank"}
  - [Joplin](https://github.com/laurent22/joplin){:target="_blank"}
  - [OneNote](https://www.microsoft.com/en-us/microsoft-365/onenote/digital-note-taking-app){:target="_blank"}
* Mock Exam
  - [OSCP Mock Exam Machines](https://github.com/six2dez/OSCP-Human-Guide/blob/master/README.md#exam-mockups){:target="_blank"}

## OSCP Exam

The dreaded 24 hours, after getting cold feet for a couple of times in booking the slot for the exam, I finally scheduled the exam. I made a backup of my VM in case something goes wrong. Read through all the rules regarding the exam and kept a backup power supply and internet. Wrote down most of the general stuff in the report
and created skeleton code for Buffer Overflow. To pass OSCP a minimum score of 70/100 is required and each machine has different points.
I highly suggest you read the [OSCP Exam Guide](https://help.offensive-security.com/hc/en-us/articles/360040165632-OSCP-Exam-Guide){:target="_blank"} for more details on what is and isn’t allowed during the exam.

To gain additional 5 points before the exam, you can submit a lab report consisting of 10 unique OSCP lab machines and a selected number of exercises from the materials.
This lab report is submitted together with the exam report.

* Machine 1 [Buffer Overflow 25 pts] -
I started my exam at 8 AM after completing the proctoring procedures and completed the Buffer Overflow Machine by 9 AM, I had parallelly taken all the screenshots and wrote vague
steps in the report, to avoid any confusion later on.

  Started to enumerate all the other 4 machines in the background, so I can save time and when I am done with the BOF machine and I can start to work on the other machines 
Immediately.

* Machine 2 [20 pts] -
Enumeration provided me the breakthrough and got the limited shell, after tinkering for 1-2 hours got the root shell by 1 PM, took a break after this machine, and had lunch.

* Machine 3 [20 pts] -
Limited shell was attained without much sweat but root shell proved to be a headache and current point total stood at 60 when the lab report 5 points are included. 
It was 6 PM and I started to get a dreaded feeling that I might not be able to complete it and I kept on enumerating further and with no luck, I switched over to the 
25 pointer and 10 pointer machines and even these machines proved futile. So took a much-needed break and came back and got the root shell by 9:30 PM. The total now stood at 70 and I was happy to gain the passing points.

* Machine 4 [10 pts] -
I had to make sure of the passing score as there is a possibility of getting the 5 points rejected If I made any mistakes on my lab report. So to get a guaranteed pass
I started to work on the 10 pointer and got the root shell by 11:30 PM. Now I was thrilled about getting 80 points and completing OSCP. All the hard work proved to be worthwhile
at that moment. Took a break and played the [Try Harder](https://www.offensive-security.com/offsec/say-try-harder/){:target="_blank"} song and enjoyed the moment for a while. 

After laying down for a few minutes almost dozed off due to exhaustion and thought to complete the exam report and started to write the report by 2 AM, I had skipped the 25 pointer machine and focused on checking whether all the screenshots are there or not, as failure to do the exam report would result in failure. I had to retake a couple of screenshots to make the report was detailed enough. After 22 hours of exam time, Everything was re-checked and the exam report and lab report was sent in the specified format. 


## Wrapping it Up

In the end, the lab report was 226 pages long and the Exam report was 40 pages long, Most of my time during the lab period was lost in completing the lab report. But in the end, I had got 75 points even without the lab report. The lab report was too long and was not worth it for the 5 points, but many were failing in the borderline 65 points and this forced me to do the lab report.

I called it a day by 6 AM, after rechecking for any mistakes and sending the reports out. It was a wonderful journey that boosted my confidence in facing any challenges in the future.

On September 28, 2020, I received the mail stating that I had passed OSCP. Almost 6-7 days after completing the Exam.

<div data-iframe-width="250" data-iframe-height="270" data-share-badge-id="6fd420ed-eafb-48ed-bff4-e442bcf5df15" data-share-badge-host="https://www.credly.com"></div><script type="text/javascript" async src="//cdn.credly.com/assets/utilities/embed.js"></script>


As of writing, I am currently vacillating between a couple of certifications and my next target from offsec side would be [OSWE](https://www.offensive-security.com/awae-oswe/){:target="_blank"}.

###  Exam Tips:

* Use Autorecon for enumeration.
* Take Breaks in between.
* Take screenshots and notes parallelly, don't wait till the end.
* Start with BOF and enumerate other machines in the background.
* Try the obvious things, without trying you won't know the output.
* Switch machine when there is no progress.
* Have proper intake of water and food.
* Having some weird idea and doubting it? try it, who knows it might work out.
* Take proper rest before the exam.
* Have proper notes, everything should be a copy-paste away. 
* Keep notes organized and have a skeleton code for BOF.
* Have an outline for the exam report.
