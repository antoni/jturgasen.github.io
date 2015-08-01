---
layout: post
title: "Tech Takeaway 016: 3 Big Shifts: CloudFoundry and the Future of Enterprise IT by James Watters of Pivotal"
description: ""
category: 
tags: ["tech takeaway", "cloud", "paas", "docker", "microservices"]
---
{% include JB/setup %}

[3 Big Shifts: CloudFoundry & the future of Enterprise by James Watters of Pivotal, presented at RICON 2014](https://www.youtube.com/watch?v=_tDla0LKhJU)

Takeaways:

* Traditional industries are being disrupted by software as evidenced by valuations (Tesla, etc)
* Traditional enterprise vendors are not doing as well as they could be, even in a strong tech economy
* The big three shifts he sees are...
	* OSS is the new open standard
	* Cloud-native app and resource patterns
	* Agile transformation of ops and dev
* Innovation teams use OSS instead of traditional enterprise vendors.  "Professional managers" are the ones that choose traditional vendors
* Cloud-native applications are unversally acknowledged as the direction you want your applications to go
* People are big fans of the **12 factor application**
* The industry is transforming from proprietary to open
* People look to communities (open collaboration) instead of companies (closed-door decisions)
* Microsoft's Docker announcement was a plea to bring Docker's community into the Microsoft ecosystem.  Note he says *community* not *product*
* Microsoft is falling behind because they don't have the community support that other projects have (eg: Hadoop, cgroups)
* In 2007 cost drove OSS usage and deployment at large companies.  In 2014 the primary drivers of OSS adoption are code quality, access to the source, and attracting and retaining talent
* Likes cloud-independent frameworks to avoid vendor lock-in
* Talks about **purpose motives** vs **profit motives**.  OSS is purpose driven while companies are profit driven
* Considers OpenSolaris one of the largest failures because you need to build a sustainable community from day one, you can't just open source the code
* Invites big companies to pair program with Pivitol to help build a community of experts who can in-turn share their knowledge
* Cloud Foundry has a foundation similar to OpenStack and it hopes to create an industry standard
* Streams logs out to the platform so developers don't need SSH access to each system
* Changing the number of instances is a simple CLI operation, allows you to scale to 300x capacity in about 30 seconds
* Shows a demo where he nukes 20% of the VMs and the system adapts in about 10 seconds
* **Choreography** instead of **orchestration**.  "Everybody at once moving to a changed state" vs a single monolithic orchestrator
* It's important to automate for both developers and operations
* Containers aren't enough.  It's where the app lives but it doesn't include all the services/platform that lives around it (logging, storage, etc)
* Cloud Foundry has four layers of HA (app, process, VM, rack/availability zone)
* The hardest part about getting enterprises onboard is convincing them that the way they wrote their app in the past (stateful, not horizontally scalable) might not be 100% compatible with a cloud-native provider
* A **firehose** is a single source for logs, HTTP events, counters, and errors - a single pipeline for everything
* Can monitor for latency increases and trigger events when they happen (restart app and send notification)
* Talks about canary deployments (deploy to 1% of servers, then 5%, then 20%, then 50%, then 100%) instead of the traditional method of a complete switchover
* Canary deployments changes the risk management profile of deployments
* Container based deployments save money from IaaS perspective because now you can subdivide VMs - your choices are not as restricted and you can put more work on a single VM
* He feels you need a PaaS to do microservices because deploying hundreds services becomes really expensive when done the traditional way
* Talks about **Hystrix** by Netflix and how it helps component failures be transparent to the overall application
* Cloud Foundry is baking Netflix OSS tools and Spring into the platform
* "You're either building a software business or losing to someone who is"
* **OODA loop** = Observe, orient, decide and act.   Should be done weekly or bi-weekly
* Talks about agile and DevOps, the usual ideas
* He thinks Docker image standard is great, but their container stuff isn't too great.  He thinks we'll always have Docker image compatability but the container will change.
* Docker containers do not persist if the supervisor process fails so it's hard to get information out of the container if the supervisor process is having issues
* There is no such thing as a Linux container.  It's an orchestration of cgroups, namespaces, and other components