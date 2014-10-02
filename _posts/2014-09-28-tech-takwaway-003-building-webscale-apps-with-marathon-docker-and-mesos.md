---
layout: post
title: "Tech Takwaway 003: Building Webscale Apps with Marathon, Docker, and Mesos"
description: ""
category: 
tags: ["tech takeaway", "mesos", "marathon"]
---
{% include JB/setup %}

[The Mesosphere and Riot Games guys talk about how to build highly-available, fault tolerant apps with Mesos at TechTalks Los Angeles](https://www.youtube.com/watch?v=6y86sw7wj_A)

Takeaways:

* We need *aggregated* resources (Mesos) instead of *partitioned* resources (VMs)
* You break up your resources (into VMs) and then put them back together (webscale apps), that seems backwards
* Instead of making the lines between machines more distinct (VMs) they want to make them blurrier (treat a cluster as one big computer)
* They want to do for the data center what an OS does for a single machine
* Static provisioning and partitioning leads to wasted resources, Mesos is more about dynamic provisioning
* Mesos is a **resource manager**
* Mesos allows multiple local schedulers to live in the same cluster (eg: Marathon + Hadoop + Chronos)
* **Resource offers** are when Mesos sends an offer to a scheduler
* It's up to the scheduler to determine which offer is a *good fit* or if the offer is a *good enough fit* or if the offer *doesn't fit*
* The offer architecture allows the scheduler itself to make the decision rather than Mesos
* Offers include details such as physical location (rack and/or cluster location) so the scheduler can make decisions.
	* Do two tasks need to be on the same rack for data locality?
	* Do two tasks need to be in separate clusters for fault tolerance
* Assume that things break all the time
* Mesos is designed to have no SOPF
* Slave processes on a node can die and re-attach (if you configure it that way)
* Frameworks are made of 2 parts: the **scheduler** and the **executor**
* Frameworks interact with Mesos and the scheduler, which is a part of the framework, makes the scheduling decisions
* Mesos is very flexible.  You can include things such as NICs and GPUs in the resource offers
* Break down jobs/tasks into categories
	* On-demand batch jobs (MapReduce)
	* Long or infinitely running tasks (webapps)
	* Scheduled tasks (cron job type things)
* Mesosphere has written frameworks that cover most types of webapps, you shouldn't have to write your own for each new webapp
* Chronos supports job dependencies
* ElasticSearch and MPI and Hadoop already and many more have schedulers written
* Marathon is like an init or upstart for your data center, generic long-running task scheduler.  ("I always want 60 instances of this webapp running")
* Marathon has a webform that makes REST calls to the scheduler
* Marathon also uses the REST interface so you can easily view task states via CLI
* Marathon supports health checks such as...
	* Interval
	* Max Consecutive Failures
	* Timeout
	* Grace period
	* Path / port / protocol to use for the health check
* Marathon has 2 ways to determine if stuff is healthy.  Either poll the endpoint or you can subscribe a web server to receive callbacks.
* When something happens a task will post events to it's callback handler and you can act on those as necessary
* Can kill tasks via CLI
* Supports HA but that's done out of band separate from Marathon / Mesos (eg: scripting against your load balancer)
* Master heartbeats to slaves
* Has an **event bus**, research this more
* Upcoming Marathon and Mesos features...
	* Versioned immutable apps including history and rollbacks
	* Health checks, they are finding ways to have to mesos-slave do the health checks because the scheduler doing everything doesn't scale
	* Health checks will be aggregated and sent back to the scheduler vs the scheduler doing 20 billion health checks
	* Rolling deploys, 2 parameters, upgrade 1 instance then 100, then 25% then 50% then 100%
	* Namespaces and dependencies
* In Marathon they have a flat list of apps right now.  They want to create dependencies (trees)
* Will be able to do things like first scale up DBs before scaling up web servers so your DB doesn't get overloaded, dependencies essentially
* Mesos could be called a **resource broker**
* Mesos is compute-agnostic, onsite vs local, can talk to EC2 and OpenStack at the same time, etc
* Largest scale to date is 50,000 slaves
* Mesos only has one active master at a time, but you can run more that are passive
* Always want a uneven number of masters because you need quorum to elect a new master
* When a new master is elected all components of the cluster will re-register with the new master and reconcile state (vs replicating state to all passive masters)
* This may change in the future (eg: replicated state between masters)
* Hadoop framework may support weights so you can prioritize things that run on the same cluster
* Yarn is geared towards big data apps, not a general-purpose solution like Mesos
* Mesos supports resource reservations (for a department or an app), implemented via weights
* Reservations can also be set on a per-node basis (eg: I want framework X to have Y resources on every node)
* Research is being done on "right-sizing" tasks, similar to FB's Tupperware
* People are working on "reactive auto-scale", where you can fire up more AWS instances if your cluster is running low on capacity
* People are also working on "preditive auto-scale" by looking at traffic patterns and pre-provisioning more nodes
* Twitter does not use any special hardware for their master