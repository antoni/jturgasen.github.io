---
layout: post
title: "Tech Takeaway 002: Tupperware: Containerized Deployment at Facebook"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[Aravind Narayanan talks about deployment, sandboxes, and containerization at Facebook](https://www.youtube.com/watch?v=C_WuUgTqgOc)

Takeaways:

* Lots of stuff can go wrong at scale!  What about...
	* Decommissions
	* Failover
	* How do you distribute binaries?
	* Geo-distribution
	* How do you daemonize the process?
	* How do you monitor the process?
	* How do you automatically add new machines to the cluster?
	* Does the machine have the right kernel/packages?
* Abstracts like Mesos/Borg/Omega so the developer doesn't have to worry about these things
* Automatically handles failover
* At Facebook...
	* Each data center is split up into **clusters**
	* A cluster is multiple racks
	* Each rack has multiple machines and a top of rack switch
	* Similar to Borg/Omega's "failure zones"
* A Tupperware **job** is similar to a service
* Each Tupperware job has multiple **tasks**, and each task is an instance of the service
* The scheduler talks to an agent, one agent per box
* There are multiple schedulers per cluster.  I wonder if they have layers of schedulers?
* When a developer wants to deploy something the write a spec file (eg: config.tw)
* Spec file...
	* Command & arguments
	* Dependencies (packages)
	* CPU, mem, disk
	* Number of replicas
	* OS version, kernel version
	* Custom commands
	* Python-based so they can use Python modules instead of direct OS commands (eg: git vs the Python git module)
	* In source control so they can see how an app changes over time, also allows sharing between apps
* The scheduler also talks to the **server DB**
* The server DB is an inventory of all the machines that FB has and their capacity (CPU cores, RAM, disk)
* This is how the scheduler determines where to run a task
* Agents download binaries by using BitTorrent to prevent bottlenecks and to create fault tolerance
* The scheduler heartbeats with the agents
* When a failure occurs it goes through the same workflow as it did when the job started to determine where to re-start it
* Technicians can notify the scheduler of impending operations (decomms, hardware maintenance) so it can start to migrate jobs/tasks off those system(s)
	* Allows graceful migrations of stateless services
	* Stateful services can endure maintenance.  eg: If the service takes 30 minutes to build an index when it starts, just shut it down gracefully for the 10 min maintenance instead of killing it
* Can create service dependencies (eg: my app needs a log aggregator service)
* Scheduler allows attributes like "I use a lot of bandwidth" so it will ensure the tasks run in different racks as to not overload the switch
* Agents...
	* Has an API, uses it to start/stop tasks
	* Agent "phones home" to the scheduler when a new machine is provisioned
	* The **agent helper** process is the parent process of your task
	* They use an agent helper so they can upgrade the agent software easily
	* Agent Helper heartbeats with the agent
	* Helper is important because without it the scheduler has no visibility into the machine and may duplicate work
	* If the helper can't heartbeat with the agent it kills it's task to prevent zombies and duplicates
	* Helper Agent also serves as a logging daemon ("log catcher"), captures stdout and stdeer and compresses it on the fly
	* On the fly compression gives a predictable performance decrease instead of spikes when the logs are rotated + compressed
* Agent Helpers start your task in a sandbox
* Used to use chroots but they suck, no real isolation
* **Bind mounts** directories into the container when needed
* Runs an SSH daemon within every container so developers can look at logs, use gdb, etc
* They turn off the OOM killer because it kills random stuff inside the container, can lead to services in a weird state
* Instead of using OOM killer the agent gets a notification and does a clean restart of the task
* Uses **adaptive limits** to set the limits on a container.  Uses historical information to make decisions.
* Remember that containers have their own namespace so some monitoring tools may break
* FB uses **service discovery** because things are so dynamic
* When a task starts, it registers.  Other tasks can look up your service.
* When a host fails the scheduler updates the registry
* When a host fails and restart it again updates the registry
* Monitoring...
	* How many clients are connected to your service?
	* How many exceptions is it throwing?
	* How long is taking to reply?
	* Engineer can define custom health checks based on these checks
	* Tupperware can take action when a threshold is reached (restart or migrate service)
* Hypervisors makes debugging harder and have a performance penalty
* Do dry runs!
* Manage dependencies!
* Roll out new stuff to 1 machine, then 100, then a cluster, then a data center, then everywhere
* Provide users with sane defaults so a coder doesn't have to read the whole spec file manual
* Schedulers use Zookeeper to store state
* Has a system that recommends spec settings based on historical trends