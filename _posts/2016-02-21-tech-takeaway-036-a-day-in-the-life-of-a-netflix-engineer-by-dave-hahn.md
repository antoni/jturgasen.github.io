---
layout: post
title: "Tech Takeaway 036: A Day in the Life of a Netflix Engineer by Dave Hahn"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[A Day in the Life of a Netflix Engineer by Dave Hahn of Netflix at AWS re:Invent 2015](https://www.youtube.com/watch?v=-mL3zT1iIKw)

Takeaways:

* At Netflix their ops team is responsible for...
	* Crisis management
	* Availability reporting
	* Reliability best practices
	* AWS relationship
	* Operations education
* The #1 goal of the team is to **protect the customer experience** by designing for...
	* Graceful degredation
	* Failover
	* Failback
* Talks about the user experience and graceful failures like...
	* Show movie list if recommendations are broke
	* Failover if a streaming endpoint fails
	* Don't show broke links in a page, use filler instead
* They trade increased velocity of innovation for increase failures (or increased risk)
* Beware homegrown tools that make it easy to do the wrong thing (eg: no or little bounds checking)
* You can't change things you can't measure
* They use A/B testing
* Blah blah blah about how great they are....
* Being good at data center operations does not help them competitively as a company.  They're entertainment, not an internet company
* Talks about their custom CDN devices
* SOA "composed of loosely coupled elements that have bounded contexts"
* They have an internal, self-documenting tool that enumerates API dependencies and associations
* Blah blah blah DevOps culture
* Each microservice team completely owns their service   (on-call, deploy it, pick tech for it, etc)
* Making devs be on-call is better than having ops people try to figure out software failures
* Include both devs and ops in incident reviews / post-mortems
* Mentions Eureka, their service discovery software
* They use the EC2 API so much that they get throttled.  To alleviate this, they cache AWS objects locally in some custom software
* Talks about Hystrix
* Talks about automating and abstracting infrastructure to make life easier for  the devs
* Talks about Spinnaker (their deployment tool)
* They collect metrics for "insights".  Uses "Atlas" (TSDB), their own software
* Talks about using preditive metrics (deviations from the norm/prediction) instead of simple threshold metrics
* Blah blah blah plan for failure in the cloud
* The policy is that "a single failed instance should not affect a running service"
* They are big into stateless-ness (in case of failures)
* Use Cassandra for "high data spread and redundancy"
* Chaos Monkey runs hourly.  It has been 2 years since Chaos Monkey has actually broke the system
* They kick off Chaos Kong monthly

Not a great talk for me because I already understand most of the concepts and tools he spoke about and he didn't dig too deep - but a good talk for newbies.