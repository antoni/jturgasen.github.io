---
layout: post
title: "Tech Takeaway 017: Deploying All Day Without Breaking the Internet by Sean O'Connor of Bitly"
description: ""
category: 
tags: ["tech takeaway", "git", "microservices"]
---
{% include JB/setup %}

[Deploying All Day Without Breaking the Internet by Sean O'Connor of Bitly, presented at RICON 2014](https://www.youtube.com/watch?v=c5PZn28BV00)

Takeaways:

* Talks about consistency via automation
* Talks about the idea of keeping everything (software + config management) in a **single repo**
	* Makes rollbacks easy
	* Each commit is a snapshot of the infrastructure at a point in time
	* Requires some organization within the repo so things don't get too crazy
* They use microservices that communicate via HTTP and async queues
* In a microservice architecture failure manifests via feature degradation instead of everything being down
* One advantage of microservices is you only change one service at a time as opposed to changing everything at once (faster, less risk)
* Microservices also make things easier to understand from an engineering standpoint because it's similar to having classes and functions inside your code
* From an operational standpoint microservices allow you to easily isolate the problem
* Microservices allow you to change a service's stacks without it's consumers noticing (eg: different tech stacks for different services)
* It also allows you to tailor the hardware to a service because you may want to scale up instead of out in some situations
* Allows you to scale services independently
* Design systems so it looks like "letters in the mail instead of two people having a conversation" (eg: message queues)
* An async-heavy design allows for strong decoupling and fault isolation
* An advantage of using message queues is you can make changes to the consumers without making changes to the sender
* Message queues also help with burst traffic because messages will pile up in the queue and will be processed later instead of overloading a service
* Async is great!  Except where you care about consistentcy.
* Their URL shorterner is sync instead of async because it's latency sensitive (customer-facing) and so in a failure scenario it can return an error instead of a short link that doesn't work
* Their metrics systems are designed async because latency isn't a concern
* NSQ is their homegrown distributed message queue (pub/sub, no SPOF, no centralized bottlenecks, dynamic config, etc)
* NSQ allows you to create high-water mark that sets the boundry between what's in memory and what's on disk (performance vs durability)
* NSQ also has a *killer* performance analysis GUI, worth checking out
* Create a centralized way to track deployments so you can tell if events/errors/alerts are correlated with a deploy
* Centralized deployment tracking also helps stop people from stepping on each other's toes
* They use a post-PR hook in GitHub that runs tests before the PR is merged
* They have a plan for if/when they get DOSed.  It moves redirects (their primary business) to another data center but lets everything else languish.
* They call code review "peer review".  They do things like examine architectural changes, determine operational changes that need to happen, and so on
* Their only rule is "whoever wrote the code can't hit the merge button"
* Bitly puts operational / deployment checklists in PR comments so ops knows how to push out and verify the new build
* Things they ask during architecture review...
	* How is the change going to be backwards compatible?  Deploys aren't instant so you will have multiple versions running at the same time.
	* Think about how to change things without a total system outage.  If you think about it up front you can usually avoid a total service shutdown/outage/restart
	* "How do we know when it's down/broken?"
	* "Once we know it's broken, how do we determine what's wrong?"  Logs, metrics, etc.  What info do you need?
	* Understand the failure modes and design for failure.
	* Where are the availability needs?  Service A might need 99.999% but service B might be ok with 98%
	* What hardware does it require?
	* Let's talk capacity
	* Data locality
	* What impact does this have on other systems?  Will you need to increase the capacity of other systems to deal with the new service?
	* What are the ongoing operational tasks?\
* They use Nagios and Graphite
* "If there's a not a Nagios check on it, it's broken and you just don't know it yet"
* They track API response times and error rates
* Use centralized logging, makes it easier to find patterns since you don't have to SSH into 9999 boxes
* Understand why you are doing things in a particular way