---
layout: post
title: "Tech Takeaway 027: Building a Website To Scale   Target: 200 Million Page Views per Day and Beyond! by Eric Pickup"
description: ""
category: 
tags: ["tech takeway"]
---
{% include JB/setup %}

[Building a Website To Scale - Target: 200 Million Page Views per Day and Beyond! by Eric Pickup, presented at ConFoo 2012](https://www.youtube.com/watch?v=RlkCdM_f3p4)

Takeways:

* When you plan capacity be sure to consider all of the **non-web stuff** that will hit the web server (API, mobile)
* When choosing technologies for you site also **consider the talent pool**.  They moved away from perl because good perl programmers are very hard to find in their area
* One big issue when creating the new site was **maintaining parity** with the old site as the old site changed.  Feature X changed on the old site, but didn't even exist on the new site
* **Purposely replicate non-showstopper bugs** from the old site to the new site so users would not notice any change
* Nice slide of the new site's architecture at 10:45
* One advantage of round-robin DNS is is that the work is done on the client
* One disadvantage of round-robin DNS is that traffic is "stuck" to a region/datacenter/cluster.  If it dies, the users are pinned to the dead region/datacenter/cluster
* They use **Varnish for device detection** (is the client a mobile phone?)
* Varnish is also used for **session management** because they can't use a back-end solution (memcache) if the request never hits the backend (Varnish)
* Another use of Varnish is "URL normalization" to reduce the size of the cache (eg: foo.com/abc?x=1 and foo.com/abc?x=2 is normalized to foo.com/abc)
* He feels that **URL normalization was a huge part of optimizing caching**
* **ESI = Edge Side Include**.  Like an Apache server-side include but at the cache-level (Varnish)
* They use Varnish's health checks so they do not need to add a layer of HAproxy between Varnish and Nginx
* Consider putting a message queue between the app and DB tiers so that writes are more of an async operation
* They HAproxy servers that sit between Nginx and Redis use two pools: a read pool and a write pool.  The write pool can failover to a replicated master if the main master fails
* Redis is their primary backend data store.  Don't forget that Redis' primary use is as a persistent data store, not a cache
* Carefully **examine your data patterns and use-case** before choosing your NoSQL solution
* If you need persistent data in Redis use AOF instead of RDB snapshots.  The difference in the amount of required disk I/O is massive (whole snapshot vs incremental)
* By using Redis as the primary backend data store and by using MySQL for reporting/analytics it allows them to normalize the MySQL data.  This is not possible on regular sites because the performance penalty for normalizing the data is too high
* Factors that delayed the launch of the new site:
	* Decisions concerning which technology to use
	* Learning curve for new technologies
	* Data transfer and restructuring (MySQL and Redis)
	* Staffing issues
* Consider the cold cache problem when cutting over from an old site to a new site