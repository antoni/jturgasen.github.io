---
layout: post
title: "Tech Takeaway 012: Cluster Management at Google"
description: ""
category: 
tags: ["tech takeaway", "mesos"]
---
{% include JB/setup %}

[John Wilkes discusses cluster management at Google in his keynote speech from MesosCon 2014](https://www.youtube.com/watch?v=VQAAkO5B5Hg)

* Failures dominate their design discussions
* Top of rack switch is a SPOF
* Power bus bars that power several racks are also considered a SPOF
* Diesel generators are a large SPOF.  They only get a short time (20 seconds) to start generating a ton of power (2 megawatts).  Test.
* What happens if a master node is down during the same time that you're doing an upgrade?
* When it comes to planning for failure, don't just plan for individual failures, also plan for multiple concurrent failures (what if this AND that is down at the same time?)
* The upgrade the OS if it needs it or not (every month or two).  Pretty good test.
* Don't consider failure a problem, consider it normal and plan for it.  Code around it.
* Keep application state externally rather than internally
* Consider application placement.  Are all instances of an app connected to a single top of rack switch or under the same power bus bar?
* What happens if a data center goes down?  Does the app keep running?
* Worry about availability first, performance second.  Get it right first before you make it faster
* They gather metrics and create charts for what they call their "bin packing algorithms".  How much capacity (CPU and RAM) on a physical system is wasted?
* Bin packing algorithms are key because without them you waste a lot of hardware
* "Resource stranding" is when you have leftover CPU in one place and leftover memory in another
* Track CPU used vs CPU allocated to help make better placement decisions.  Do the same for RAM.
* They found that they often utilize much less than they allocate.  They overcommit a lot.
* They do not overcommit the user-facing front end latency-sensitive work
* Things like "the interns MapReduces" are prime targets for overcommit
* They do not overcommit their external-facing VMs
* In general there are two types of jobs/workloads: Short lived (MapReduces) and long or forever running (web services)
* For service jobs, make your scheduler think harder about where to place them to avoid failures (data center, rack, cluster, power bus, etc)
* On the other hand, for fast / short jobs, don't have your scheduler think too hard about where to place it
* They don't dedicate machines to a particular workload.  There is no "GMail VM".  The VM runs whatever is placed there by the scheduler
* Applications can interfere with each other in places that the OS can't control: eg: the L3 CPU cache
* You could fix this particular problem with "cache partitioning" but x86 and Linux don't support it
* They monitor "cycles per instruction" (CPI)
	* Gather CPI for all the tasks in a job
	* Find outliers (ones that are hurting / taking too long / being impacted)
	* Take action.  He seems to hint that they kill tasks of a batch job that are killing performance because you can always restart the task somewhere else, it's batch)
* They wrap stuff in containers (I assume cgroups)
* They link their applications statically so when the OS gets upgraded everything keeps running
* Like Mesos, each job/app has a config and declares what it wants (1400 copies running at all times)
* Job/app configs are integrated via their source control system (audit trail)
* They use quotas/limits/QoS for their jobs.  Eg: Your MapReduce isn't as important as making sure search is running smooth)
* "If you're not monitoring something, it's out of control."  Not "out of control" like crazy, but you literally lose control over it
* Every single process (OS level process) has a little web server running inside it to display metrics.  They then aggregate and display it.
* They use "bittorrent-ish" stuff to distribute an app's binaries.  Distributing binaries to 2,000 nodes from a central host is pretty slow.
* You can only SSH into machines that are running one of your tasks, not every machine
* Even the KVM VMs live inside a container
* They use containers for resource isolation, execution isolation, and CPU QoS
* Make things "share fate".  Keep the web server and the what they call "log roller" together.  If one dies, both should die.
* A collection of containers that get scheduled together (grouped together) wherever they land is called a "pod"
* They stick "labels" on pods (key-value pairs)
* You can use labels to label your pods with things like "this is a frontend app" or "this is a backend app" "or this one is a prod prod"
* The other advantage to labels is you can have your master scheduler go find all pods with label "frontend" or whatever
* You can apply multiple labels to the pods your app uses (frontend + prod, frontend + test, backend + low_priority, etc)
* "Replica controller" allows the engineer to set the number of pods (replicas) that their application needs
* Labels seem to be hierarchical.  The number of replicas is on the top and below that is the attributes / labels of the pod
* Describe the end state and let automated systems get you there
* Every pod gets it's own IP
* When it comes to calculating oversubscription, do it dynamically instead of statically.  Monitor / trend utilization and decide on the fly.
* Put in triggers / limits for auto-scaling and resource utilization.  Memory leaks suck.
* Be careful with labels / job descriptions / attributes.  Google has 238 possible labels.  That's too many.