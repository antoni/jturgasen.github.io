---
layout: post
title: "Tech Takeaway 008: Treating Your Infrastructure Like Garbage by Mike Fiedler"
description: ""
category: 
tags: ["tech takeaway", "chef"]
---
{% include JB/setup %}

[Mike Fiedler from Datadog tells us how to treat our infrastructure like garbage at ChefCon](https://www.youtube.com/watch?v=2s2ql6qcM2Y&list=UUxEieNpB_tXiUBoF9zkPmAw)

* Monitor services, not servers (uptime, performance, alerts, etc)
* Don't get attached to your server, "no server hugging", cattle instead of pets, design for failure
* Choose elasticity.  Distributed, self-healing storage (Cassandra, H-Base, Riak, mongoDB, Redis)
* Design web tier to tolerate failure (duh).  What about session state, where is it stored/replicated?  eg: store stuff in an clustered in-memory web-tier behind your web
* Use your load balancer's health check features.  Determine what constitutes "health" in your environement / stack
* Don't health check too often and overwhelm your servers!  eg: health checks talk to web that talks to DB and overloads the DB
* Plan for the Slashdot / HN effect
* What happens if you lose component X of your system/stack
* What percent of capacity should you run at?  90% to save money?  50% to counter traffic surges?  At what percent do you auto-scale?
* Be sure to monitor all the tiers (LB, web, app, memcache, db)
* Make your monitoring elastic too!  Will the monitoring scale when the site scales?

Mostly pets vs cattle, loose coupling, and scaling.