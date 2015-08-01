---
layout: post
title: "Tech Takeaway 021: Embracing Failure: Fault Injection and Service Resilience at Netflix by Josh Evans"
description: ""
category: 
tags: ["tech takeaway", "cloud"]
---
{% include JB/setup %}

[Embracing Failure: Fault Injection and Service Resilience at Netflix by Josh Evans, presented at North American Network Operators Group (NANOG) 61](https://www.youtube.com/watch?v=9R710ry-Cbo)

Takeaways:

* His team focuses on driving quality and velocity
* Failures happen all the time - design for it
* **Handle your exceptions well**
* **Isolate faults**, build fault tolerant systems if possible
* **Design for fallbacks and degraded experiences**
* Once you're in a good state how do you stop from drifting back into failure?
* Unit testing, integration testing, stress/load testing, etc
* Testing increases confidence, but it's never enough
* They inject failures into *production* in order to validate their resiliency
* **Forcing failures on a regular basis** allows you to allows you to remove uncertainty
* They use internal predictive mechanisms to help determine traffic peaks and can auto-scale preemptively
* They use **Hystrix** as a "circuit breaker"
* Every command that goes out is wrapped in a "dependency object" via Hystrix
* If certain **failure thresholds** are met, the circuit breaker kicks in and services stop sending requests to the failed service
* State is problematic because it takes more time to spin up replacements, states must be saved to persistent storage and/or distributed
* Lessons learned from intentionally taking down a data center...
	* Large scale events are hard to simulate (eg: taking out a data center / availability zone)
	* Load balancers need to expire connections quickly
	* Lingering connections to caches must be addressed
	* Not all clusters / AZs are pre-scaled for additional load
	* Some of their apps were not configured for cross-zone calls
	* There were mismatched timeouts (Hystrix timed out faster than the LBs)
	* REST client "preservation mode" prevented client failover
	* Cassandra worked as expected
* They use "regional load balancers" on the frontend
* **Zuul** (smart traffic shaping) sits behind the regional load balancer
* They use georouting at the DNS level to load balance
* They use **Latency Monkey** to inject and simulate latency at the service level
* **Startup latency** is different than runtime latency (pulling down JAR files, libraries, etc)
* **Service owners need to know all of their dependencies**
* Dependencies change over time, stay up to date!
* Fault injection is great but you can still deploy bad code or make human errors
* They use CI/CD and are increasing code coverage
* Netflix does "automated canary analysis" and also use staged configuration changes (think canary for configuration management)
* Context engages partners (data and root-causes help people get on board)
* You create better solutions through collaboration than you do alone
* Chaos Monkey runs daily
* After the very first Chaos Monkey run some teams opted out of the second run so they could make more resilient services based on what was found during the initial run
* If you use tools like this on a regular basis you won't see as much drift