---
layout: post
title: "Tech Takeaway 030: Cloud Native Cost Optimization by Adrian Cockcroft"
description: ""
category: 
tags: ["tech takeway", "cloud", "aws"]
---
{% include JB/setup %}

[Adrian Cockcroft of Netflix discusses AWS cost optimization at re:Invent 2014](https://www.youtube.com/watch?v=LZwlkqERv2g)

Takeaways:

* You can measure cost in 3 ways:
	* **Bottom Up** which is the cost of all compoents (VMs, storage, ops costs, etc)
	* **Product** is the cost delivering and maintaining a product
	* **Top Down** total IT budget divided by the number of components
* Top down and bottom up will never match because you forgot about
	* Hidden subsidies
	* Hidden costs
	* The subsidies tend to be bigger than costs
* Time to value matters because getting your return offsets your costs
* Cloud-native cost optimizations are
	* Get the project done quickly so you can get value faster
	* Once you get it into production you can then see where the real costs come from (eg: needs expensive SSDs so optimize disk I/O, etc)
	* Turn things off when you don't need them
	* Buy capacity only when you absolutely need it
	* Consolidate workloads, especially on reserved instances
	* Plan for price cuts
	* Use FOSS tools!
* Overprovision on day 1, shrink later
* Cloud allows you to experiment (eg: they set up a thing in Brazil, it didn't work, they shut it down.  Not much $ lost, just some time)
* Talks about underprovisioning during lauch, mad scramble to get more capacity, etc  VS   overprovision and waste money
* Things to turn off
	* Production capacity during off-peak hours
	* Test environments during off hours
	* Dev environments when the software developers aren't using them
	* Data science clusters during off hours
* Use spot instances for fault-tolerant batch jobs (Hadoop, drug discovery/simulations, etc)
* Because Docker starts so fast, using it allows you do snapshot/freeze test environments when they are not in active use
* Assuming you use Docker and it's super fast the devs can freeze dev/test/QA when they get coffee or lunch then quickly resume when they return
* Consider seasonal savings (eg: Target/Walmart can reduce capacity for non-Black Friday/holidays)
* "Autoscale the costs away" - Netflix saved 50% by using **reactive autoscaling**
* **Predictive autoscaling** saved them about 70%, see the Scryer blog post on the Netflix tech blog
* Use the **AWS Trusted Advisor**
* Other simple AWS tips:
	* Dump unused EIPs
	* Deleted unassociated EBS
	* Delete old EBS snapshots
	* Use S3 object expiration
* Look at Netflix **Janitor Monkey** to clean up unused stuff
* *When* do you pay?  How long is the building/servers/AC unused until the private cloud goes live?
* Do the math and compare reserved instances vs using on-deamand for a long time
* You can consolidate AWS billing among multiple accounts
* Consolidating billing can help you reach volume discounts
* With consolidated accounts **reserved instances are shared across all accounts** (eg: you can use your prod reserved instances for test if traffic is low)
* You can amortize reserved instances
* The downside of reserved instances and consolidated accounts is the location (region) and instances (size) are fixed
* There is a **reservation marketplace** where you can buy and sell unused, partial reservations
* Consider Amazon's technology refresh (old is M1 and M2, new is M3)
* Use reserved instances as API/HTTP/etc servers during the day and then have them run Hadoop/batch jobs at night
* Netflix has a metric that is essentially "unused reserved instances", that's how they know which instances to run Hadoop on at night
* Check out Netflix's **Ice** tool for cost analysis