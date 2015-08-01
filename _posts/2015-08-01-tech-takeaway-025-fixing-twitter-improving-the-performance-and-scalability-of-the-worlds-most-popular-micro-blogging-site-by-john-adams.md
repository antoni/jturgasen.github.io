---
layout: post
title: "Tech Takeaway 025: Fixing Twitter: Improving the Performance and Scalability of the World's Most Popular Micro blogging Site by John Adams"
description: ""
category: 
tags: ["tech takeaway", "logging", "git", "monitoring"]
---
{% include JB/setup %}

[Fixing Twitter: Improving the Performance and Scalability of the World's Most Popular Micro-blogging Site by John Adams, presented at Velocity 09](https://www.youtube.com/watch?v=9X_ed6GPofQ)

Takeaways:

* They use **bare metal** instead of clouds in order to get more accurate metrics
* Use metrics to make actionable decisions
* Find the weakest point, improve it, repeat with the new weakest point
* **Instrument everything** - the more metrics the better (usually)
* Some metric collection can impact performance
* Collect, graph, and report *critical metrics* as **near real time** as possible
* Put time of last deploy on your monitoring status page
* Consider putting **time of last deploy** at the top of all graphs so people can easily see the impact
* Examine last 100k of aggregated logs and look at 500 or 503 errors per second.  If it's > X it's time to call in ops
* An alternative term for "dark launch" is "darkmode" (feature flags and emergency stop button all in one)
* Any time a feature flag is flipped the stakeholders are automatically notified
* Use configuration management right when you start - it's huge
* They use peer-review / two sets of eyes for infrastructure changes and review them using **Review Board**
* Pre-commit hooks check Review Board comments for a string like "reviewed by John" before committing
* Apache: MPM model, MaxClients, TCP listen queue depth
* They use Varnish and configure a thread limit
* Think about how you deal with slow performance.  How will you fall-back when the system is overloaded
* Their attack plan for performance issues is symptom, bottleneck, vector, and solution
* Example: Slow search is the symptom, database is the bottleneck, delays are the vector, and the solution is to optimize DB queries
* Examine caching and cahce invalidation
* Disk is the new tape.  It's slow.  Web 2.0 isn't possible without lots of memory / caching / SSDs.  Without them, response times would be terrible
* Create separate memcached pools for different data types of prevent eviction
* FNV hashes are faster than MD5
* They moved their memcached interaction out of Ruby and into Native C
* **Invalidating caches** at the right time is difficult
* Consider the impact of memcached clusters and/or instances failing
* **Evictions make caches unreliable** for data such as darkmode / dark launch flags
* Cache and control abusive clients
* Analyze and normalize data to increase caching
* You don't need ACID DBs for everything!  How much consistency do you need?
* They use a message queue for all inter-daemon communications
* Durable message queues create huge performance problems at scale
* Analyze the request pipeline and determine which operations do not have to be synchronous (e-mail, complex object generation, 3rd party services)
* **noatime** on file systems!
* Monitor your DB for slow / poorly designed queries (MySQL slowquerylog)
* Use ``mkill`` to kill long running MySQL queries
* Keep users in the loop by having a status page.  Host it somewhere else (Twitter hosts theirs on Tumblr)
* Key points...
	* Databases are not always the best store
	* Instrument everything
	* Use metrics instead of guesses to make decisions
	* Don't make services dependent
	* Async when possible
* Great talk!