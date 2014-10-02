---
layout: post
title: "Tech Takeaway 005: Best practices for PaaS, OpenStack, & cloud adoption"
description: ""
category: 
tags: ["tech takeaway", "openstack", "best practices", "paas"]
---
{% include JB/setup %}

[David Linthicum talks about PaaS, OpenStack, and cloud adoption best practices at the 2014 Red Hat Summit](https://www.youtube.com/watch?v=gKHUduAD2Hw)

Takeaways:

* Create architectures that you can replicate
* "Hire someone with a brain" or get a consultant, you only need a couple to get the org moving in the right direction
* But, you must empower them!
* Most cloud based systems lack architecture
* Typically people don't have the range of skills to make good decisions
* One of the biggest problems is organizations lack architectural discipline and don't solve the right issues
* Organizations don't like to listen to consultants who say they have architectural problems
* Companies that succeed with cloud...
	* Test like crazy
	* Invest in the technology
	* Invest in people
	* Invest in training
* Problems he sees with bad clouds...
	* They don't scale
	* Insecure
	* Little or no governance
	* Creates complexity rather than reduces
	* Creates more silos
* The results of bad clouds...
	* Inefficient utilization of resources
	* Resource saturation
	* Lack of elasticity and scalability
	* Lack of security and governance (hard to retrofit!)
	* Frequent outages
	* Bad or no tenant management
* "Manage by magazine" = chasing buzzwords
* You can't iterate your way to success in a project that's long term and required for strategic success
* Core principals for a cloud architecture team
	* Use things that are cost effective and open
	* Create flexible and agile solutions (architect for change/iteration)
	* Use an actual methodology, don't wing it
* Again, security and governance are almost impossible to retrofit
* DevOps is not technology, it's ops and devs working together, processes & culture
* Security should be managed as a domain, not by ops + devs.  Get professionals!
* The best practiced deveopers he sees always rely on a security team
* Automate the infrastructure so developers can do their jobs better
* Multi-hypervisor use is becoming the norm
* Use abstraction to manage multi-hypervisor environments, he calls it "resource governance"
* By using multi-hypervisor managers you can start to extend and grow them to manage multiple cloud types (pub, priv, hybrid)
* Architect towards managing all of your clouds and hypervisors under a single "pane of glass"
* IT will always be hybrid, plan for it
* BI and data analytics are great targets for PaaS because they have a lot of repeating functions
* OpenStack is oriented towards low-state or stateless applications
* Beware moving apps into the cloud.  Can your apps...
	* Take advantage of cloud-native features?
	* Can they auto-scale?
	* What about security and governance?
	* There's lots of moving apps the the cloud, but with time you'll just have to re-architect them
* Still early in the enterprise PaaS days
* Make sure your IaaS provider plays well with other IaaS providers because you'll probably end up with multiple ones.
* Make sure your PaaS integrates with all types of environments and vendors so you don't end up with 2 or more PaaS (one for each environment)