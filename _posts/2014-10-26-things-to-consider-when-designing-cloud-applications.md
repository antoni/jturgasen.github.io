---
layout: post
title: "Things to Consider When Designing Cloud Applications"
description: ""
category: 
tags: ["cloud"]
---
{% include JB/setup %}

Nothing exciting, just a list of things to think about when designing applications that run in the cloud.

* Create fault tolerance at the application level.  Assume you don't have RAID / dual NICs / HA software, etc
* How do you manage state (eg: user sessions)?  Can you make it stateless?
* You will scale horizontally, not vertically
* Can your application handle auto-scaling?
* Your application won't run on a single system, it will run on many
* When a failure occurs, consider retry transparently 
* Use loose coupling so if one component of your system fails it won't bring down the system as a whole
* Use a shared-nothing design
* Which communication protocols will you use (IPC / named pipes vs HTTP / REST)
* Assume no two applications will be on the same server (eg: Apache + Tomcat on the same VM)
* Assume file systems on a VM are *not* persistent storage
* Don't log to the file system, design so they can be sent to a remote system or DB for persistent storage
* Use open standards and protocols
* Beware cloud vendor lock-in.  Will it work on Rackspace and AWS and OpenStack or does it make provider-specific API calls?
* Assume you will have a cache tier (memcached)
* Plan to use eventual consistency
* Build in monitoring and expose it via API
* The network may be private, but maybe not.  Encrypt all communications if possible.
* Do you need to encrypt at-rest data?  If so, which data?  Classify it.
* Consider concurrency.  You might have 100,000 customers hitting 500 servers.  How do you keep things in sync?
* How do you upgrade or change your application's settings?  Design so you can change every/most settings while it's running and upgrade without down-time
* Make static content cache-friendly
* Build in throttling mechanisms.  These can also be used for SLA / QoS
* How close does the data need to be?
* How does your application react to long distances and latencies (DB in NYC, app tier in San Francisco, web tier in Tokyo)
* Make your application easy to deploy and install, automation friendly, configuration management friendly
* Shard and/or federate your data
* Can you interact with your application in a standards-compliant way (HTTP, REST, etc)?
* Are your APIs stable and/or versioned?
* Design for failure.  What if one app server fails?  What if they all fail?  What if the DB is answering very slowly?
* [Embrace the Chaos Monkey](http://techblog.netflix.com/2012/07/chaos-monkey-released-into-wild.html)
* Consider the user experience during failure scenarios.  HTTP 500 pages are ugly.