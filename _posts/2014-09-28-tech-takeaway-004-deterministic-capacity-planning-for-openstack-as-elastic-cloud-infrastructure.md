---
layout: post
title: "Tech Takeaway 004: Deterministic capacity planning for OpenStack as elastic cloud infrastructure"
description: ""
category: 
tags: ["tech takeaway", "openstack", "capacity planning"]
---
{% include JB/setup %}

[Keith Basil, Sean Cohen, and Tushar Katarki talk about OpenStack capacity planning at the 2014 Red Hat Summit](https://www.youtube.com/watch?v=zVBA6URb54M)

Takeaways:

* Virtualization is scale up (larger VMs), elastic cloud is scale out (more VMs)
* In enterprise virtualization you pick the VM size, in OpenStack you can only pick from a pre-determined list
* Neutron gives out IP, default route, firewall settings, default group (not sure what default group is)
* Storage from Cinder (block devices)
* Glance gives the URI of the OS image to Nova
* Make operations reproducible and programmatic
* Record performance history or else you can't plan well
* A service's performance isn't measured by average speed but by the consistency of speed
* **Fractional proportions** is how big providers cut up their VMs.  XL -> Big is half of XL -> Medium is half of Big -> Small is half of Medium
* This gives you very efficient capabilities for scheduling and placement
* Break the instance families (hardware configs) down into different types (general purpose, memory optimized, CPU optimized)
* Be sure to gather developer requirements so you can understand what they need (huge amounts of RAM, etc).  What are they used to?
* Does your customer want to copy AWS/Google Compute, or do they want to choose their own hardware configurations?
* With storage are you optimizing for performance (IOPS+latency), throughput, or cost + capacity?  Always tradeoffs.
* When optimizing storage for capacity there are no IOPS guarantees and it's billed by size and I/O usage
* When optimizing storage for performance there are guaranteed IOPS
* You can also use a blended approach to storage: IOPSs scale with volume size, billed by size only.  In other words, performance scales with capacity.
* Break your private cloud storage up into tiers: Performance optimized (SSDs), throughput optimized (SAS drives), and general (large capacity, general purpose)
* In Cinder you set volume types (performance, throughput, general, etc)
* When doing storage capacity planning be sure to account for RAID and replication to determine usable storage
* When creating a performance optimized tier the IOPS should scale *lineraly* with VM count
* For performance optimized tiers limits should be seen as triggers for storage scaleout (if latency goes up it's time to add more disks)
* For networking do you want to optimize for throughput, resiliency, or latency?  Always tradeoffs.
* For a cloud you don't want to use a traditional network model (switches -> switches aggregated -> core routers)
* Instead, you want to use a **spine and leaf** topology for uniform bandwidth and elasticity
* With spine and leaf any node should have equaldistant access to another node (eg: nodes on the "edge" should not have to go through extra hops, all should be the same)
* The only time they put storage on it's own network was because of security requirements
* When using spine and leaf you are only bound by the port density of your switches when scaling
* If you use up all your ports you can use what's called a **substage** or **multistaging**
* Once your AWS bill hits about $25k/mo you might want to DIY it at a colo, especially one that has "Amazon Direct Connect"
* By doing this you have like 80% of your capacity and burst to Amazon when necessary
* Use VM placement planning to help determine how many IPs you need.  100 servers times 64 VMs (max) per server = 64k IPs
* QoS also lives in Cinder drivers so it's not JUST Cinder providing QoS (and failing because the driver ignores it)