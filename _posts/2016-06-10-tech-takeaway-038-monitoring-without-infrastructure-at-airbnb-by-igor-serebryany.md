---
layout: post
title: "Tech Takeaway 038: Monitoring Without Infrastructure at Airbnb by Igor Serebryany"
description: ""
category: 
tags: ["tech takeaway", "monitoring"]
---
{% include JB/setup %}

[Monitoring without Infrastructure @ Airbnb by Igor Serebryany at SREcon15](https://www.youtube.com/watch?v=LwILywmtL_I)
 
Takeaways:

* Create a monitoring *platform* instead of just a bunch of tools (more later)
* They want engineers to move fast "and ship things"
* Deployed almost 10k times in 2014
* They consider their dev pipeline their most important monitoring metric
    * Code is the product so find ways to get it out faster/better
    * They monitor and optimize built time
    * They also monitor and optimize how long it takes code to get into staging
    * and into production
    * They try to keep the overall pipeline at under an hour
* 2 pillars of how to ship code fast
    * Trust the engineers (nobody *wants* to break prod, etc)
    * Empower the engineers with tools such as...
    * Feature flags, CI, automated deploys, comprehensive monitoring (he means metrics), general alerting
* 3 pillars of monitoring
    * Unified monitoring (for SRE sanity)
    * Usable by all engineers - simple interface
    * Deployed just like others apps
* Unified monitoring / monitoring platform is important or else you have 9999 different monitoring solutions
* Make sure your alerting tool has an API so you can automate things like adding a new node, updating thresholds, etc
* If you use an on-premise solution make sure it plays well with configuration management
* Airbnb uses...
    * ELK
    * NewRelic
    * DataDog
* Beware log entries that have PII, don't send them to a 3rd party
* They use Logstash tagging
* Each service communicates via it's own "front end" HAproxy.  They can tag the HAproxy logs and then see the flows between services / HAproxies
* Logstash lets you "preprocess" log lines so you can manipulate them before they are shipped and stored
* NewRelic is "the most important component of the infrastructure for enabling engineers"
* NewRelic essentially sits inside your app and has a view of everything it's doing
* Each Airbnb service (think microservice) has a NewRelic agent inside it
* This allows engineers to easily interface with and gather metrics without having a different platform for each service
* Mark deploys on your dashboards so you can see if perf changed after a deploy
* Talks about how NewRelic has good SLAs and Airbnb saved $ and time by buying monitoring rather than creating it
* They pull CloudWatch metrics into their DataDog dashboards
* Use whatever monitoring app you want so long as it has the features you need and it's automatable
* Says to use syslog over UDP instead of TCP but doesn't say why
* They created "abstraction wrappers" to ensure NewRelic is installed with every app, and in a consistent fashion
* Essentially, they automatically wrap apps in the NewRelic monitoring so devs don't have to think about it
* Talks about how they automatically set up monitoring at provisioning time
* They expose statsd info via localhost IP so apps can pull metrics from statsd
* DataDog alerts to PagerDuty
* Alerts should...
    * Automatically be applied to new hosts
    * Be as few in number as possible
    * Be very descriptive - explain problem AND provide steps
    * Be consensually created
* Their monitoring framework will query the AWS API each hour, pick up new hosts, and add the appropriate monitoring to them automatically
* Talks about how alerts are sent directly to the developers when appropriate
* Monitor and instrument your monitoring
* They recommend UDP for log/metric communication to **localhost**.  This is so your app doesn't crash by getting hung up on retrying TCP connections
* I have no idea what the above line means, he didn't have a good data flow diagram so I don't know where it fits in
* They have alerts for low-traffic situations (if we are only getting 30% of our normal traffic, something is wrong)
* Talks about how the DataDog agent is a "buffer" during network problems (stores/queues metrics until the connection is re-established)
* Their ElasticSearch AWS nodes cost tons of $$ because they want to keep metrics cached in RAM for faster searches
* They rotate this in-RAM data for 3 weeks, then they send things out to long-term archival
* They use Chef Solo!
* They have an inventory system.  It talks in JSON
* Logstash tags logs based on info from the inventory system

Some great tricks in this one!