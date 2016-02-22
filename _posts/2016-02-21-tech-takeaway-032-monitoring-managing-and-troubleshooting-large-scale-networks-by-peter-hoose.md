---
layout: post
title: "Tech Takeaway 032: Monitoring, Managing, and Troubleshooting Large Scale Networks by Peter Hoose"
description: ""
category: 
tags: ["tech takeaway", "networking", "cloud"]
---
{% include JB/setup %}

[Monitoring, Managing and Troubleshooting Large Scale Networks by Peter Hoose of Facebook at NANOG 64 in 2015](https://www.youtube.com/watch?v=BRY9xwg5nAU)

Takeaways:

* Must take risks to make progress
* Embrace hacks - sometimes they're necessary
* Myths about large networks
	* Automation fixes everything (it's a tool, addon, augment)
	* We've fixed everything (Google/FB/Twitter's network)
	* This talk doesn't apply to you (it does)
* **Microbursts** are rapid bursts of packets that lead to failures (pack loss, RSTs, TCP retrans, etc)
* Determine which counter/metric will confirm it's a microburst, then isolate the problem (does it only happen on switch X and not the others?)
* They determined certain ports would burst to line rate in parallel
* Note that they 30 second average counter on the port did NOT show this behavior: they had to sniff the traffic to see it
* How bursty is each flow?
* Lessons learned
	* Resolved issues (gj)
	* They got RCA (good)
	* They used software to fix the problem (ok)
	* Service owner identified the problem - engineers/ops should know before customers
	* Took a long time to resolve (weeks/months)
	* Small loss can cause significant impact on a service
* Link imbalance is the next topic....
* They had a single link that would spike to 100%, the other 3 were normal
* First thing they did was shut down the link, see what happens..and it moved to a link on a completely unrelated pair of switches
* **Fat flow** is when you shut down a link, and the problem moves because the traffic is still flowing but now it simply uses a different path
* A fat flow would explain the link getting pegged, but the other 3 links in the bond had LESS traffic while the link was pegged
* Problem is only on backbone devices, only happens when a link is >80% utilized, and the problem was migratory
* They tried something where they changed the hashing method that determines which link a flow goes over...**worked for a few hours**
* They then wrote some software to monitor for the issue and fire when it was found
* It sent message to FBAR (cronjob on steriods, auto-remediation) -> roll hash -> works for a few hours -> continue monitoring
* They made a list of
	* What they were going to examine/measure and
	* What they expected to see for each object
	* eg: TCP retrans should back off due to congestion, etc etc
* **For some reason, TCP tried harder during congestion rather than backing off**
* For some reason, open connections increased during the congestion, and fell off when the congestion stopped
* They then determined that the connections went from caching layer to DB layer
* Their caching layer used a **MRU** caching policy
* This means last conn that had a reply is the one where the next data is sent.  If that last connection is slow/congested.....
* It keeps trying harder and harder
* So they changed from MRU and that fixed it, done
* They did the usual for network monitoring: alert if >X% loss or latency
* They started at 5% loss and moved the threshold down with time
* **1% TCP packet loss can result in 50% reduction in throughput**
* 0.00010% loss can reduce throughput by 50% on 200ms RTT flows
* They talk about the various TCP algos and how they deal with and how fast they recover from loss/congestion
* Monitor interfaces for loss.  It's super easy to fix and less packet loss is better
* Their interface monitoring workflow is automated - when it finds one, it takes it out of service and puts in a ticket with the hardware techs
* He also notes that all these great things were NOT accomplished with politices, change controls, process, etc etc - just teamwork and software
* FBAR processes 3.37b messages per month.  1% are real problems
* FBAR auto-remediates (FaceBook Auto Remediation?)stuff.  It resolved 99.6% of problems
* You need 150 engineers to equal FBAR
* When a fan on a router dies, FBAR gracefully redirects/bleeds traffic off, then takes the router out of service (vs overheat and fail)
* How do they determine if interface is down?  Good old syslog
* Watch for BGP state, scrape devices for BGP and OSPF state, etc
* They use SNMP
* After gathering all of this info, they simple write software that would emulate what a human would do with the info
	* What actions would a human take with this info?  Script that.
* They also use pattern detection in addtion to the usual threshold -> alert