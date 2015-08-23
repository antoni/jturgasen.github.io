---
layout: post
title: "Tech Takeaway 031: The State of the Art in Microservices by Adrian Cockcroft"
description: ""
category: 
tags: ["tech takeway", "microservices", "cloud", "devops"]
---
{% include JB/setup %}

[Adrian Cockcroft of Netflix talks about microservices at Geekdom in SF on January 15th 2014](https://www.youtube.com/watch?v=pwpxq9-uw_0)

* Talks about using the OODA loop (observe, orient, decide, act) in continuious delivery
* Innovation is in the **observe** step
	* Observing custom pain points
	* Observing the competition to plan a competive move
	* Observing customers via measurement
	* Observing the market for a "land grab" opportunity
* The **orient** step is about analysis
	* Use big data
	* Model your hypothosis
	* Look at data that no-one has ever looked at before (eg: find new data sources)
* The **decide** portion of the loop is
	* Slowed down by corporate culture (do you need permission from a VP?)
	* JFDI (just fucking do it)
	* Share your plans, get a response, maybe take action (slow)
* The **act** part of the loop should
	* Sped up by the cloud
	* Deploy things quickly (eg: don't submit a ticket and wait for a month)
	* Improve features incrementally (agile)
	* Use AB testing
	* Deploy daily!
	* Use feature flags
* **The faster you go through the OODA loop, the faster you learn about your customers and the market**
* Note that the OODA loop isn't always a loop (eg: sometimes you need to go back to Decide if the Act doesn't work)
* Talks about how silos work (sysadmin, storage, PM, network admin, software devs, etc)
* Monolithic product delivery spans multiple silos
* Each product team should use microservices, have an API, and the products should interact using those APIs
* A product team consists of a PM, UX, developers, QA, and DBA.  Note that sysadmin, network admin, storage admin are not included (see below)
* You need commonality in delivery (eg: a platform)
* The platform team can be internal (sysadmins + network admins + storage admins) or can be external (AWS, Rackspace, etc)
* Essentially your ops team becomes your platform team and they are mostly coders (automation, APIs, "glue", etc)
* DevOps is a re-org due to the points above (silos, new ops/platform team, new product teams)
* DevOps is not a tooling problem, it's a cultural and organizational problem
* He feels Dev and Ops should work for the same management
* A platform is a set of patterns that everyone is buying into and a common platform makes stuff easy to do
* Netflix originally had a monolithic app
* One problem with the monolithic model is that it doesn't scale
	* If there's a showstopper bug how do you determine who introduced it?
	* A single bug breaks the entire monolith
	* Friday afternoon = there's a bug, everyone stop all your work and figure it out (huge waste of time)
* Microservices is a model that stops the developers from being blocked
* In the microservices model each service has their own PM, their own release model, their own developers, and potentially their own programming languages
* Microservices decouples development cycles
	* Monolithic is a single development cycle
	* Microservices is many individual development cycles
* He loves Docker because it deploys so quickly (build + deploy in seconds)
* Microservices APIs must be stable!
* Take advantage of the pre-built containers on Docker Hub for common application (Apache, Redis, etc) and layer your configuration on top of them
* A huge advantage of the pre-build Docker Hub containers is there is lots of people using them, thus lots of people are testing them for you
* The entire Docker deployment process (compile code, extend container, deploy to PaaS) takes seconds
* The speed of Docker is addictive, once devs use it they won't go back
* With microservices and Docker the
	* Cost, size, and risk of change is reduced, thus
	* The rate of change increases so you are more competitive as a business
* He feels that microservices + CD via Docker is a true disruptor
* Microservices is **loosely coupled service oriented architecture with bounded contexts**
* The original service oriented architecture (SOA) got bogged down with heavy weights stuff.  Microservices are lightweight
* If you have to update everything at the same time you aren't loosely coupled
* If things can be updated independently then you are loosely coupled
* Tight coupling can be an **organization** problem too (if you have to coordinate between many parts of the organization)
* Database schemas can create tight coupling.  If multiple components depend on the schema you need to bounce them all at once
* If you have to know too much about the surrounding services you don't have a bounded context
* Each microservice team is trying to build what is essentially an exeternalizable API that is stable and self-contained
* This means all the developers need to do is
	* Know about *their* API
	* Don't break the API
	* Make it better on inside (optimize, add features, etc)
* You can decompose a monolithic system into microservices by
	* It's the job of management to break big problems into smaller problems and give them to engineers to solve
	* Read the book Domain Driven Design by Eric Evans
* Coupling concerns and tips:
	* The code will resemble the organization that wrote it (monolithic or microservices).  Build an organization that resembles the code you want built
	* Enterprise Service Buses (ESBs) are terrible because everyone gets locked into them (common message formats, etc)
	* It's better to have point to point messages (API to API) instead of a common message bus
	* Message buses tend to suffer from split-brain and CAP theorm bites you
	* Beware centralized DB schemas
	* Make your versioning flexible and allow for multiple versions running at the same time
* One great thing about the speed of containers is you can run them on VMs that run for minutes or hours and you can kill them when you're done or when a new version spins up
* This leads to cost savings when using AWS or a utility computing model
* Why is your test and dev environment running when nobody is at work?  Kill it, save money, spin it up in the morning
* The next step beyond Docker is **AWS Lambda**
* Lambda is essentially containers than run your code, spits back the result, and kills itself.  Billed in 100ms increments
* Great slide with links to microservices architectures for various companies at 31:15
* Shows the "template" for a microservices architecture at 32:52
* If it's ephemeral you can Dockerize it
* Check out **Gilt's** microservices architecture.  It's a lot like Twitter's but extended and uses Docker
* Don't use message queues for everything because they aren't the best with failure modes and corner case failure modes
* The characteristics of web scale:
	* Brand new microservices are deployed infrequently (plan ahead!)
	* New versions of existing services are deployed frequently or automatically
	* You don't need general purpose orchestration that can deploy everything
	* You will use hundreds of microservices
	* Each deployment is heavily customized (he didn't elaborate on this)
* Talks about Docker monitizing the app store (sell pre-made containers, rent them by the hour, create pre-made environments, etc)
* At Netflix they run Cassandra in a container and it sucks its data from other containers
* He sees 3/4 of new projects being written in Go / GoLang
* Read the book **Lean Enterprise** (DevOps, CI/CD, and more)
* How do you incrementally get to microservices?
	* **First** break up your monolithic backend data store
	* Find something that's "unrelated" or a one-off in the backend data store, split it off, repeat
	* Create a data-access service layer that sits in front of all the DBs. Never access the DB directly
	* Storage Tier as a Service over HTTP (STaaSH) is a Netflix OSS tool that does this, supports MySQL and Cassandra backends
	* **Second* break up the front-end
	* Put an API proxy tier in front of it (eg: Netflix Zuul, Apigee, etc) and start splitting up the traffic
	* The API proxy tier essentially acts as an abstraction layer
* When building a new microservices architecture your first step should be the API proxy tier (Netflix Zuul, Apigee, etc)
* Have the API proxy tier reply with static/throwaway responses until the real APIs are built
* The data schema for an application is part of the application (don't have some separate group do it)
* Instrumentation is baked into reusable components.  Netflix OSS tools log in a standard way (standard columns)
* **Zipkin** does end-to-end tracing / metrics
* Netflix has 600 microservices
* A problem with a large number of services is how do you visualize and monitor individual service dependencies instead of all 600
* They control access to services/APIs by using AWS security groups
	* Each service has a SG with the same name
	* The service is put into the SG
	* Any services that want to call that service are put into the SG
* They also use AWS IAM and AWS key/secret management
* For small teams monolithic is ok so long as you can split it later (eg: if you add a new feature do you want to destabalize your existing codebase or just create a new microservice?)
* On a small team "one container should be one person's work"
* "There is no good way to do API versioning but there are less bad ways"
* If you build an API build the client library as well because if you don't then the consumers or other programmers will
* The API client library needs to efficientely encode and decode the payload and do proper error handling
* API client libraries should be very self-contained
* "SDN is the data center people trying to build Amazon VPC"
* Keep a consistent platform because the cost of forking it (eg: moving from Java to Python) is high because you will then need to keep them in sync