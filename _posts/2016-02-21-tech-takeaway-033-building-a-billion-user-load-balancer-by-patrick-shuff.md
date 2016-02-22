---
layout: post
title: "Tech Takeaway 033: Building a Billion User Load Balancer by Patrick Shuff"
description: ""
category: 
tags: ["tech takeaway", "networking", "cloud"]
---
{% include JB/setup %}

[Building A Billion User Load Balancer by Patrick Shuff of Facebook at SCALE 13x in 2015](https://www.youtube.com/watch?v=MKgJeqF1DHw)

Takeaways:

* Requests that come into FB are broke into two categories
	* Dynamic (newsfeeds, likes, etc)
	* Static (video, image, js/css)
* Lots of crap that nobody cares about....
* Overview of TCP/IP....
* Their front-end L7 load-balancer ("proxygen") terminates SSL and TCP and then reverse proxies that back to web server - standard stuff
* They have a L4 load balancer sit in front of their L7 load balancer
* Their L4 is called "shiv" and is based on the "ipvs" (IP virtual server) module
* The L4 makes a hash for each connection and the hash is used to "sticky" the connection from the L4 to a consistent L7 (vs bouncing between L7s)
* But wait, they have ANOTHER LB in front of the L4
* Their final LB is the TOR router (L3)    TOR/L3 lb -> L4 lb -> L7 lb -> http servers
* ECMP = Equal Cost Multipathing   the TOR routers use this
* FB's typical cluster is 10s of shivs, 100s of L7 behind that, and thousands of servers behind the L7s
* **Everything in prod is bare metal, FB does not use virtualization**
* L4s get ECMPed from the TOR
* The L4s peer with the TOR via the Python OSS **exabgp** daemon
* They **do** keep state between the TOR and L4 so that clients consistently hit the same L4 + L7
* As long as they L4 servers have a consistent view of the L7 servers, clients will get that consistent hash + sticky and won't have to re-create the TCP connection
* Keeping the TCP connection open is feature of "ipvs"
* For static requests, **direct server return** is important
* Egress packets bypass the L4 and go directly to the client
* ipvs/L7 does IP in IP encapsulation for packets sent to the L4. they are sent via a tunnel interface (not the normal eth0)
* They used to re-write MAC addresses instead of doing IP in IP, but that's because their TOR used to be L2 but now it's L3
* Talks about POPs and edge locations, reducing how far a packet has to travel...
* HTTPS Soeul to Oregon it takes about 1.1sec to actually get data from FB....slowwwww
* So they put a POP in Tokyo.  Now Korea goes there
* Now it takes about 330ms
* They have pre-established connections between edge and FB
* They did a lot of TCP tuning at their POPs - no need to tune clients when things terminate at the POP and the real action is POP <---> FB
* TOR and L4 and L7 **live in the POP and in the data center**, servers are in the data center
* FB has a DNS LB called **Cartographer**
* Cartographer ingests information (capacity, latency, routing, health), processes it, and makes a "dns map" that is then used to create real, optimial, updated DNS entries
* They use **tinyDNS**.  The updates from Cartographer come every few minutes
* They do DNS LB not based on geographic closeness, but by network/logical closeness
* For static resources, 1 out of every X requests (X is a big number) for static content will be purposely sent to a non-optimal POP
* They then trace this request to see the path and time it takes.  This is how they map "closeness"
* Because Cartographer is dynamic, it allows them to direct traffic to each POP so that it's highly (but not over) utilized
* They use a 5 min TTL for facebook.com .  POP dies = 5 min outage from customer's perspective
* Says people (clients) don't respect TTLs, beware
* By using open resolvers (aka Google's DNS) you might not get the optimal POP because FB sees the request as coming from Google, not from you
* Their HHVM dynamically generates links within the pages its generating.  These links point to content on an optimal POP to optimize performance