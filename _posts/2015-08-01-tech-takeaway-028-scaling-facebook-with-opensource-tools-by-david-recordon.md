---
layout: post
title: "Tech Takeaway 028: Scaling Facebook with OpenSource Tools by David Recordon"
description: ""
category: 
tags: ["tech takeaway", "logging", "monitoring"]
---
{% include JB/setup %}

[Scaling Facebook with OpenSource Tools by David Recordon, presented at FOSDEM 2010](https://www.youtube.com/watch?v=HAzeJTgFwDc)

Takeaways:

* Facebook originally sharded their DBs based on school (one for Harvard, one for Stanford, etc)
* I have 100 friends and I view my timeline.  FB must get 100 pieces of data and more (eg: comments by non-friends on my friend's post).  This means they might be pulling from thousands of sources
* Also consider that some pages may need to pull from **millions of sources** (eg: Coca-cola and their millions of followers)
* They use a fairly standard architecture: LB -> web -> API/memcache/database architecture
* FB uses PHP because it's simple to learn, simple to write, simple to read, and simple to debug
* They also like PHP because you don't have to wait for it to compile before you can view your changes
* One issue is **high CPU usage** because they pull data from many sources and the data must be assembled after it's gathered
* **HipHop** transforms PHP into highly optimized C++ code.  Note that HipHop is depreciated as of 2013 and **HHVM** is it's successor
* Use an optimizer to look for dead code branches that won't be used or executed
* Questions to ask yourself if you are considering creating something as a service/API
	* How much overhead is required for maint, deploy, and the new code base?
	* A new service creates a new failure point
	* Can you create frameworks that are common to all services?
* Their backend infrastructure uses a variety of languages.  Use the right language for the job
* FB's **Thrift** (now Apache Thrift) is essentially a language-agnostic RPC server
* By using Thrift developers do not need to worry about serialization, connection handling, or threading
* Thrift has bindings for almost every language
* FB uses **Scribe** instead of syslog
* The idea behind Scribe is it's a **multi-tier fan-in system**
* Scribe batches messages before sending them cross-datacenter
* Syslog info is stored in a **Hadoop + Hive cluster**
* Note that Scribe is no longer updated by FB, consider **Apache Flume** or a similar alternative
* Every "site" (API, service, etc) must have logging enabled
* If your site uses photos be sure to **think about which size(s) you need** (eg: original size + thumbnail + fixed size, etc)
* Originally they stored files on NFS servers and shared them via HTTP but it didn't scale because...
	* File systems aren't great at supporting huge numbers of files
	* Limited by I/O and not by storage density
	* Photo metadata was too large to fit in memory, thus each read required many disk operations (photo + metadata vs just photo)
* Next, to scale photo serving even further, they did the usual stuff
	* Cache high volume small images
	* Use a CDN
	* Cache them in memory at origin
	* Enable NFS file handle caching to reduce storage tier metadata overhead
* When the files were stored on NFS they had to look up 3 pieces of metadata for every file (directory inode, directory data, file inode)
* Instead of 4 I/O operations for each photo they wanted to reduce it to 1 (photo ID that's held in RAM + photo data from the disk)
* FB created **Haystack** to reduce photo serving overhead
* Hadoop is complex and hard to learn so they use **Apache Hive** on top of Hadoop in order to create a simpler user interface
* Great diagram of their data flow architecture at 23:32
* They break Hadoop into multiple clusters (one for Scribe, one for production data, an Adhoc query cluster that replicates data via Hive from the production cluster, etc)
* memcached is crucial to scaling
* But beware, memcached is easy to corrupt, it has a limited data model, and it's **inefficient for small items**
* They use MySQL because it uses a shared-nothing architecture
* FB does not use MySQL for it's relational features and they do not do JOINs on the server.  This is because of their specific use case, not because the features are unstable
* **JOINs are done on the web server**
* When choosing infrastructure solutions they **test 2 or 3 at a time and choose the best one** rather than deciding on one solution and sticking with it hell or high water
* They make data consistency decisions on a per-service basis
* Photo **resizes are done on the client** as part of the upload process
* Use CDNs for images, CSS, and JavaScript
* They use **Ganglia**