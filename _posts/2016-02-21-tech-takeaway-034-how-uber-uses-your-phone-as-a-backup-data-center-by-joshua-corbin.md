---
layout: post
title: "Tech Takeaway 034: How Uber Uses Your Phone as a Backup Data Center by Joshua Corbin"
description: ""
category: 
tags: ["tech takeaway", "cloud", "disaster recovery"]
---
{% include JB/setup %}

[How Uber Uses Your Phone as a Backup Data Center by Joshua Corbin of Uber at Scale 2015](https://www.youtube.com/watch?v=0EhTOKcwRok)

Takeaways:

* Talks about the general architecture
	* Clients' app talks to DC
	* Driver's app  talks to DC
	* Client request -> DC -> driver app
* **State change transtion** is the process of requesting a driver, the driver accepting, and so on
* They have to handle the trip data for the duration of the trip.  Can't delete it before the driver drops the passanger off!
* Talks about how if a data center fails you lose your trip data because it's not replicated to another DC
* They tried replicating trip data like normal (DC to DC replication)
	* But subject to lag, gets hard with >2 DCs, requires a lot of bandwidth, etc etc
* **Rather than save trips to a data center, they save it to the driver phone**
* If a DC fails, the driver's app reconnects to a different DC and re-uploads the data - the phone is the source of truth
* Assume the driver's phones are weak - lock down the data yourself and/or be picky about what data you send to the driver's phone
* KISS for the replication protocol
* Minimize bandwidth used by the mobile network
* Their replication protocol is a simple K/V format
* For this system they decided on...
	* Ensure system is non-blocking but eventually consistent.  Don't block business operations
	* DC failover/failback without worrying about stale data
	* 4x 9s reliability
* Replies to the client are async so they're non-blocking (eg: no need to wait for true success re: replication service)
* How do they deal with state and moving between data centers?
	* Tomestones for completed trips are kept on the phone, phone is the source of truth
	* Tomestones are great because they are just a few bytes vs lots of data for the full trip
	* They use **vector clocks** to compare data on phone vs data on server and then sync if different
* They tested data center failover and iterated based on what they learned
* **Thundering herd problem** is when you fail DCs and the new DC gets a big rush of traffic, has cold caches, and gets crushed
* Key concepts to get 4x 9s
	* All mutations that are done by the dispatching service are actually stored on the phone
	* Data that's stored on the phone can be used for restoration
	* Ensure the backup data center can handle the load
* They gather lots of metrics (driver phones, how well the mutations are stored, etc)
* They then compare the data they gathered to the data that their service expects it to have
	* Is it in sync or out of sync?
	* How out of sync?  Why?  etc etc
* They can break down metrics by app version
* Metrics are also sent to the backup data center.  They are then used for a **shadow restoration**
* After the shadow restoration at the backup DC, they compare the trip info that was restored to a snapshot of trip data from the primary DC
* Are they the same?  Are they different?  Exactly what is different?
* This analysis allows them to refine their replication so the RPO is as small as possible