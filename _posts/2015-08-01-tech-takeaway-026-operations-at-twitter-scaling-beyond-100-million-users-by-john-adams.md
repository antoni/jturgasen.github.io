---
layout: post
title: "Tech Takeaway 026: Operations at Twitter: Scaling Beyond 100 Million Users by John Adams"
description: ""
category: 
tags: ["tech takeaway", "logging", "monitoring"]
---
{% include JB/setup %}

[Operations at Twitter: Scaling Beyond 100 Million Users by John Adams, presented at Large Installation System Administration Conference 2010 (LISA '10)](https://www.youtube.com/watch?v=z8LU0Cj6BOU)

Takeaways:

* 75% of their requests come via API and about 25% via web.  API is growing, web is shrinking
* When a user connects to the web site it sends about 1 meg of HTML and JavaScript.  From then on it's almost all JSON API calls via AJAX
* If you display links the JavaScript itself needs to be secure
* Nothing works the first time!
	* Scale the site using the best available technologies
	* Plan to build everything more than once
	* Most solutions work to a certain level of scale but then you must re-evaluate to grow
	* This is a continual process
* UNIX stuff fails at scale!
* **cron** jobs cause "micro outages" when a job kicks off at the same time on all machines
* You can fix the cron issue multiple ways: Random sleeps, stagger the jobs, MD5 hash and grab the last few characters, etc
* **syslog** is another pain point
* Messages are truncated at about 1500 characters because they are larger than the MTU of a UDP packet
* Expect data loss when the syslog server becomes overwhelmed
* Beware data rounding over time in time-series / RRD databases.  Things like one minute outages may be rounded out so you can't actually see the outage when you look back
* They increase performance / scale by using the following method:
	* Find weakest point using metrics + logs + science
	* Take corrective action
	* Repeat
* **MTTD** stands for "mean time to detect" a problem in the infrastructure
* They design for failure.  They can lose a "significant portion" of the infrastructure and the site will remain online
* Talks about DevOps tools and ideas, the usual stuff
* Instrument everything - collect tons of metrics.  They monitor 30k or 40k data points, but beware because too much monitoring can impact the system
* Their ops guys do a lot of data analysis
* Reporting tools should report critical metrics in as near to real time as possible
* Be sure to monitor and report API availability
* They do a lot of **profiling**.  Latency, memory leaks, network usage
* For network level monitoring they love **yconalyzer** by Yahoo and also use tcpdump + tcpdstat
* How fast are TCP connections being opened and closed?  What's the SYN rate?  What are the packet sizes?
* They also use Google's **gperftools**.  They have custom modifications that allow gperftools to look inside Ruby
* Use configuration management as soon as you can - it saves sooo much time later
* They use Puppet + SVN
* Make use of **post-commit hooks** and have them kick off consistency checks (automated QA)
* **Nobody logs into machines directly** - they use CM tools only
* They use an in-house tool to hold their machine database (asset database)
* Twitter uses an in-house tool named **Murder** for deploys.  It is bittorrent-based rather than a centralized server or tiers of servers
* Murder is built with Python + libtorrent
* By using a bittorrent based deploy system they can update >1k machines in 30-60 seconds
* Murder talks to the in-house machine database to determine which machine(s) to deploy new code to
* To minimize errors they use **Review Board** so each change is reviewed by a person other than the person who made the change
* They use SVN post-commit hooks to call the Review Board API, publish the diffs on Review Board, and then it sends an e-mail to everyone in operations to review the change
* Twitter uses **canary deploys**
* Syslog doesn't scale because....
	* No redundancy or ability to recover properly if the daemon fails
	* Moving large files around is painful
	* Instead they use the Facebook tool **scribe**
* In Twitter's infrastructure they use LZO to **compress logs in real time and then send the data Hadoop + HDFS**
* Logging to Hadoop + HDFS is very powerful because it's an easy way to collect logs in a "central" location and then run analytics against the logs using MapReduce
* Run check scripts both during and after a deploy
* They use Google Analytics on their error pages so they can determine how much availability was lost based on how many people accessed the error pages
* Their dashboard has a "criticals" view for important graphs
* Share analytics with everyone (ops, dev, business)
* Watch 500 and 503 errors per second to catch problems before they get out of control
* **Block deploys if the site is in an error state**
* Display time of last deploy on your dashboard
* Communicate deploys to teams via IRC / Campfire / HipChat
* Graph time-of-deploy along side server CPU and latency
* **Draw lines in graphs when deploys go out**.  This makes it easy to see if a deploy caused a problem (increased latency, traffic dropped after the deploy, etc)
* Every new feature must be wrapped in a feature flag.  They call this **darkmode**
* Their feature flags are not binary.  They **use a 1 to 10000 scale that determines what percentage of servers to deploy to (10000 = all, 100 = 1% etc)**
* Feature flags / darkmode also acts as an emergency stop button
* They can **put services into static/read-only mode** where instead of hitting a backend service the request will instead get static content from a cache
* Great picture of their request pipeline at 29:13
* Analyze the request pipeline, determine the bottleneck, tune, repeat
* Consider DB connections a "scarce commodity" that shouldn't be used often
* They use a queue system for their web -> app tier instead of just having web throw traffic at the app tier and hope it doesn't backup
* They **segement their memcache clusters into pools** for better performance (eg: memcaches that hold session data are not in the main pool)
* memcache evictions make the cache unreliable for important stuff (darkmode flags)
* When storing consistent data in memcache (eg: each item is 10 bytes) monitor memcache's slab allocation and watch for high use / eviction rates on individual slabs using **peep**.  Adjust slab factors and size accordingly
* Monitoring TCP retransmits
* People often waste a lot of memory because they use it with the default parameters
* Some talk about decoupling/microservices/decomposition and it's usual advantages
* Think about which services can be taken out of the "real time" pipeline and performed async (send a SMS or e-mail, etc) by moving the work to queues
* They abstract services using **Apache Thrift**
* They use a sharding framework / library.  This abstracts sharding away from both dev and ops
* "Disk is the new tape" because disk is way too slow for Web 2.0 responsiveness
* Use FNV as your hashing algo in memcache because it's much faster than MD5
* Test and see what happens during a memcache outage
* Consider the**cold cache** problem.  What happens after a system failure?
* Mounting your DB with atime enabled will kill your performance
* Automatically kill long-running SELECT queries by using **mkill**
* It's better to kill the long-running queries rather than let them bring the whole site to a crawl
* In general any SQL query that's **longer than a couple seconds is a bad thing - it won't scale**
* **Their key to scalability is collecting extensive metrics (instrumentation)**
* Collecting metrics is an iterative process.  Start small but keep adding more.
* If the site fails and you can't figure out why, it's probably time to add more metrics
* If you use bleeding-edge tools don't forget that **you will be debugging them** as much as the tool's developers
* Determine how many web servers you can lose before the site is unusable.  Sent an alert only if X% fail rather than sending alerts for each individual server
* Same for DB read slaves
* They use a very fast monitoring timeout (eg: if a request takes >50ms mark the server as dead)
* "It's a dance between system failure, timeouts, and monitoring"
* Post-commit hook that looks for the string "reviewed by valid_user_name"
* Their code reviews can be a couple hours or a couple days.  Being **agile is great but reliability is more important to them**
* They have an automated process that creates a change request (post-commit hook?)
* When their provider provisions a new machine they receive an e-mail.  They parse and ingest the e-mails into their machine database and Puppet
* They use templated router config files.  This allows them to create tools that work against those templates to inject configuration changes