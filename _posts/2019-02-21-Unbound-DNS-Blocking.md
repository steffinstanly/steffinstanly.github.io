---
layout: single
title: "Unbound DNS Blocking"
header:
  teaser: oth.png
  overlay_image: unbound-dns/unbound cover.jpg
  caption: "[__Unbound DNS Blocking__](https://www.nlnetlabs.nl/projects/unbound/about/)"
related: true
comments: true
---

Today we will learn how to create our own recursive DNS server using Unbound. 
This will improve performance through caching. We will also look at blocking unwanted pages.

Download the Official Unbound DNS files from the __Github__ Repository which is given here "[__NLnetLabs-unbound__](https://github.com/NLnetLabs/unbound)"

{: .notice--warning}
__WARNING:__ I am by no means an expert in Unbound DNS! I tried to explain about Unbound DNS setup the best way I could, and I'm sure I might have made a few mistakes here and there. So if a mistake was made or something is misleading, then please let me know in the comments section!

Step 1: Install Unbound

The Unbound package is included in the base repositories for most Linux distributions, installing separate repositories is usually not necessary.

ON UBUNTU 

```console
# apt-get update && apt-get install -y unbound
```

ON CENTOS 

```console
# yum install -y unbound
```
Step 2: 
ON UBUNTU 

Change /etc/unbound/unbound.conf:
```console
include: /etc/unbound/unbound.conf.d/*.conf
```

ON CENT OS 

Change /etc/unbound/unbound.conf:
```console
include: /etc/unbound/conf.d/*.conf
```

Next we will enable unbound

<a href="/images/unbound-dns/unbound-1.png"><img src="/images/unbound-dns/unbound-1.png"></a>

After package has been installed, make a copy of the unbound configuration file before making any changes to original file.

```console
# cp /etc/unbound/unbound.conf /etc/unbound/unbound.conf.original
```

 Next, use any of your favourite text editor to open and edit ‘unbound.conf‘ configuration file.

```console
# vi /etc/unbound/unbound.conf
```

## Enable IPv4 and Protocol Supports

Search for the following string and make it ‘Yes‘.

```console
do-ip4: yes
do-udp yes
do-tcp: yes
```

## Enable the logging

To enable the log, add the variable as below, it will log every unbound activities.

```console
logfile: /var/log/unbound
```
## Hide Identity and Version

Enable the following parameters to hide id.server and hostname.bind queries.

```console
hide-identity: yes
```
Enable following parameter to hide version.server and version.bind queries.

```console
hide-version: yes
```

Include the block file path to the unbound.conf file to setup the block list for unbound.

<a href="/images/unbound-dns/unbound-2.png"><img src="/images/unbound-dns/unbound-2.png"></a>

{: .notice--info}
__NOTE__: Change the resolv.conf IP to the interface IP if you are running Unbound Locally in your system 
     
Add the system or server ip to the interfaces list

<a href="/images/unbound-dns/unbound-3.png"><img src="/images/unbound-dns/unbound-3.png"></a>

By default, the port is 53 if change is required just edit the port number

{: .notice--info}
__NOTE__: check whether any other process is using that port using the command __netstat -tunlp__  

<a href="/images/unbound-dns/unbound-4.png"><img src="/images/unbound-dns/unbound-4.png"></a>

Next, Change the access control for our interface IP network

<a href="/images/unbound-dns/unbound-5.png"><img src="/images/unbound-dns/unbound-5.png"></a>

Forward zones is where the IP is forwarded after the making the request to our local server. After the request has been made to our server the recursive call is made to the ISP’s DNS server.

<a href="/images/unbound-dns/unbound-6.png"><img src="/images/unbound-dns/unbound-6.png"></a>

After making the above configuration, now let's verify the unbound.conf file for any errors using the following command

```console
# unbound-checkconf /etc/unbound/unbound.conf
```

<a href="/images/unbound-dns/unbound-7.png"><img src="/images/unbound-dns/unbound-7.png"></a>

After verifying the file without any errors, you can safely restart the ‘unbound’ service and enable it at system startup.

```console
# systemctl start unbound.service
# sudo systemctl enable unbound.service
```

<a href="/images/unbound-dns/unbound-8.png"><img src="/images/unbound-dns/unbound-8.png"></a>

## Blocklist File

{: .notice--info}
__NOTE__: Inside the block.conf always start with __server:__ on top of the file

The Blocklist can be created as a static list or you could fetch the website list from various repositories which cater sites list. One such list which i found really useful was "[__StevenBlack__](https://github.com/StevenBlack/hosts)" Github Repo where he has a well-curated block list from various sources. 

All the Block list entries are having the same format, so we can use our own custom scripts to fetch the list and change it to our need. For updating our list daily we could run our script as a __cronjob__

<a href="/images/unbound-dns/unbound-9.png"><img src="/images/unbound-dns/unbound-9.png"></a>

## DNS CACHE SETUP
 
 __Test DNS Cache Locally__
 
 Now it’s time to check our DNS cache, by doing a ‘drill’ (query) on ‘india.com‘ domain. At first the ‘drill‘ command results for ‘india.com‘ domain will take some milliseconds, and then do a second drill and have a note on Query time it takes for both drills.
 
```console
drill india.com @192.168.0.50
```
<a href="/images/unbound-dns/unbound-10.png"><img src="/images/unbound-dns/unbound-10.png"></a>

As you can see in the above output, the first query taken almost 262 ms to resolve and the second query takes 0 ms to resolve domain (india.com).
That means, the first query gets cached in our DNS Cache, so when we run ‘drill’ second time the query is served from our local DNS cache, this way we can improve loading speed of websites.

## Flush Iptables and Add Firewalld Rules

We can’t use both iptables and firewalld at same time on same machine, if we do both will conflict with each other, thus removing ipables rules will be a good idea. To remove or flush the iptables, use the following command.

```console
# iptables -F
```
After removing iptables rules permanently, now add the DNS service to firewalld list permanently.

```console
# firewall-cmd --add-service=dns
# firewall-cmd --add-service=dns --permanent
```
After adding DNS service rules, list the rules and confirm.

```console
# firewall-cmd --list-all
```
<a href="/images/unbound-dns/unbound-11.png"><img src="/images/unbound-dns/unbound-11.png"></a>

## Managing and Troubleshooting Unbound

To get the current server status, use the following command.

```console
# unbound-control status
```
<a href="/images/unbound-dns/unbound-12.png"><img src="/images/unbound-dns/unbound-12.png"></a>

## Flushing DNS Records

To check whether the specific address was resolved by our forwarders in unbound cache Server, use the command given below.

```console
# unbound-control lookup google.com
```

<a href="/images/unbound-dns/unbound-13.png"><img src="/images/unbound-dns/unbound-13.png"></a>

Sometimes our DNS cache server will not reply to our query, in the mean time we can use  flush command to remove information such as A, AAA, NS, SO, CNAME, MX, PTR etc.. records from DNS cache. We can remove all information using flush_zone this will remove all informations.

```console
# unbound-control flush www.google.com
# unbound-control flush_zone bing.com
```
To check which forwards are currently used to resolve.

```console
# unbound-control list_forwards
```
<a href="/images/unbound-dns/unbound-14.png"><img src="/images/unbound-dns/unbound-14.png"></a>

now it’s time to restart the network using following command.

```console
# /etc/init.d/network restart
```

## Setting Up Apache Server to serve the Block Page

let's make a directory for keeping our block page files and for the log files

```console
# mkdir -p /var/log/httpd/blocking.com
```
Lets Uncomment 404 page and add /index.php or the path from /etc/httpd/conf/httpd.conf for serving our custom page

<a href="/images/unbound-dns/unbound-15.png"><img src="/images/unbound-dns/unbound-15.png"></a>

Now lets make the necessary changes in our block page and start our apache server

```console
# systemctl restart httpd.service
```
Finally if we start our unbound DNS Server and browse to any website which is included in the block list we will be greeted with this __Block Page__

<a href="/images/unbound-dns/unbound-16.png"><img src="/images/unbound-dns/unbound-16.png"></a>






