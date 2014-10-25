---
layout: post
title: "Tech Takeaway 006: Scaling systems configuration at Facebook by Phil Dibowitz (Chef)"
description: ""
category: 
tags: ["chef", "tech takeaway"]
---
{% include JB/setup %}

[Phil Dibowitz talks about scaling Chef at Facebook at ChefCon 2013](https://www.youtube.com/watch?v=SYZ2GzYAw_Q)

* They manage "10s of thousand of nodes"
* Ask yourself the following when scaling configuration management...
	* How many homogeneous systems can you maintain?
	* How many heterogeneous systems can you maintain?
	* How many people do you need (to maintain the configurations)?
	* Can you safely delegate delta configurations?
	* Can service owners (eg: developers, app teams) own their own their own configurations?
* It should be deterministic (eventual consistency is not acceptable, must be complete after first run)
* Should be idempotent, eg: it only makes the necessary changes
* The design should be extensible to both internal systems and customer-facing system
* It should adapt to your workflow
* Service owners / dev teams / ops teams should know what they need  (eg: vip + shared memory + core file location + no NSCD etc), but not how to configure them
* Let the owners express what they want (VIPs, sysctl settings, etc) as data (arrays, hashes, etc)
* When expressing their needs as data it should be platform-agnostic (eg: Devs don't need to know if they are running on CentOS or Ubuntu)
* Should support rapid prototyping so you can test & push quickly
* Templatize your configs as a hash (shmem.max = 1234, core.file.location = /var/crash etc)
* Use sane defaults in your templates
* The advantage of templates is that the devs / ops / service owners can override only what they need to
* Another advantage is you don't have 9000 sysctl.confs for 9000 different (but mostly similar) systems
* ``node.save()`` doesn't scale, sends back too much data (a few hundred kilobytes)
* The standard answer of "disable ``ohai`` plugins" sucks.  So....
* The solution was ``whitelist-node-attrs`` (on GitHub).  Only send back the attributes you care about.
* Facebook's desired workflow....
	* API that allows engineers / ops / service owners to extend configs by munging (changing) data structures (diff sysctl settings for my app but not others, etc)
	* Engineers don't need to know what they are building on, just what they need to change
	* Engineers can change their systems without fear of changing anything else
	* Testing should be easy
* The API seems to talk to cookbooks directly.  Devs send their hashes to the API and it puts them into a cookbook
* Engineers only "deviate from the common case" (eg: change what they need to)
* Idempotent records can get stale (eg: remove this cron job after 01/01/14, but no human ever goes in and removes it)
* For example, you add a cron job do your cookbook, push it out, put an action to delete the cron job in the cookbook, and then nobody ever cleans up the delete code.  This sucks.
* Consider having Chef analyze the hardware and set sysctl settings based on the hardware
* Use booleans to enable/disable functions (eg: IPv6)
* DSR = Direct Server Return.  Incoming connections come through load balancer, replies go around the load balancer directly to the client.  Called nPath on F5 LBs
* They use "stateless" Chef servers (re-image at a moments notice, no search or databags)
* They separate their Chef servers out into failure domains
* Each cluster use a tiered model.  Stateless front-end Chef servers with CouchDB/Redis/Postgre on the backend.  Load balancers sit between back and front, and between front and the clients
* 2 backends in an active/passive config
* Use Food Critic for Chef correctness (eg: lint for Chef)
* Chef management is done on a per-cluster basis.  Fault domains but no "world view"
* On the clients they have a "report handler" that shoots info into their internal monitoring system (last exception, run success/failure, number of resources, time to run, system info, etc)
* Erlang rewrite gave insane performance and scalability gains
* Can use Hosted Chef's tenantcy for testing.  New bookbooks?  Upload them to a new org and then test.  Your very own test org.
* FB has a wrapper script to accomplish this
* Lessons learned:
	* Idempotent systems > idempotent records because nobody ever cleans anything up
	* Delegating delta configs means easier heterogeneity
	* Full languages > DSLs
	* Scale is more than just number of clients
	* Easy abstraction is critical to delegation
	* Testing against real systems is useful and necessary.  Not your first test, but **a** test
* That's it!

Great ideas in this talk!