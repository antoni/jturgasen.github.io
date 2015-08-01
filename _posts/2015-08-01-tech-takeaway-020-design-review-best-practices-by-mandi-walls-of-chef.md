---
layout: post
title: "Tech Takeaway 020: Design Review Best Practices by Mandi Walls of Chef"
description: ""
category: 
tags: ["tech takeaway", "logging", "monitoring", "security", "best practices"]
---
{% include JB/setup %}

[Design Review Best Practices by Mandi Walls of Chef, Presented at SREcon14](https://www.youtube.com/watch?v=eV0RcGCzgc4)

An absolute boatload of great ideas in this talk!  Takeaways: 

* For new projects talk to development before it's too late to make changes to ops
* Talk early enough to have an influence
* Don't get into the weeds, focus on what alleviates the most headaches
* Set your goals for the project.  How will you deal with...
	* Initial deployment of the app
	* Runtime management and day to day ops
	* Releases and upgrades
	* Handling outages
* Use checklists, don't depend on your memory
* Checklists are living documents, make changes as you learn new things and launch new products
* Investigate layer by layer.  Different layers require different focus and have potential to cause different problems
* Determine SSL requirements
* Look at geographic topologies
* Determine DNS requirements
* Don't let SSL certs and DNS expire
* Document the *intent* of your technical decisions
* Design systems that are easy to redeploy
* How does your web tier access other tiers?  What exactly does it access?
* What's necessary for the application to run (modules, libraries)
* What ports?  What protocols?
* What services does your app tier talk to?
* Design for failure (user sessions)
* Think about the dependency flow for releases (eg: app team releases app monthly but library team releases their libraries quarterly)
* How do you scale the app tier?
* Which changes require restarts?
* Think about whitelisting / firewall rules.  Can you easily add more capacity or will firewall rules get in the way of the new servers?
* SOX?  HIPAA?  PCI?
* Are your DB schemas versioned?
* If you do master/slave, where is the master and why?
* Data management should be automated (backups, compression, expiration)
* Is the app tier susceptible to queuing (eg: connection pools filling up)
* What are the schedules for data imports/exports/ETL?
* Do you have contact information for upstream providers?
* Where do you store data?  What's it's intended size?
* How do you measure a successful import/export/ETL?  Clean stop/start, logs.
* How do you package exported data?
* What's the schedule for exported data?
* Is the exported data a live replica or is it a dump + export?
* Does the data export impact production services?
* What's the SLA of the external API you're accessing?
* What's the account name and owner of the external API?
* Who do your contact if the external API you use fails?
* Design for graceful degradation.  Users shouldn't notice.  Log a message so your alerting system knows about it.
* When something degrades it should have a reasonable timeout and should also have a hold down (eg: wait 1 min to retry, then 2, then 5.  this way no flood)
* When there's a failure who can disconnect the code from a broken service?  Who makes the call?
* What's your logging policy?
* How do you do backups?  Do you need to do backups or can you just reprovision?
* Which OS version do you need?
* Is your restore procedure and data up to date?  You don't want to restore gigs of old unused stuff
* A stack trace is not an alertable event
* If a log is worth writing to disk, someone should know what to do with it
* What's the release schedule?
* If you need to restart the data center what order do you need to restart services in?
* All software artifacts should ship production ready.  Layer dev/test configs on  top of that, not inside of it
* Set organizational standards for software and artifact repos
* Make sure all build dependencies are available in all repos for all environments - epsecially when you are depolying to multiple geographic locations
* Create processes that make installs repeatable and reliable
* Performance regression should be part of the testing process
* What tunables exist and what are the indicators that we must change them?
* Document known failure patterns and indicators
* Which software dev is on call?
* Who is oncall from the product team / business side?
* You should have a health check that checks the entire stack, but it should not disrupt the stack
* Consider creating static failover pages in place of 500 errors.  Make it non-cachable
* Have empathy!