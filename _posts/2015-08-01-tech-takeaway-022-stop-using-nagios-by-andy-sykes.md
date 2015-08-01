---
layout: post
title: "Tech Takeaway 022: Stop Using Nagios by Andy Sykes"
description: ""
category: 
tags: ["tech takeaway", "monitoring"]
---
{% include JB/setup %}

[Andy Sykes talking (or perhaps ranting) about why we should all stop using Nagios at the London DevOps meetup on February 12th, 2014](https://www.youtube.com/watch?v=Q9BagdHGopg)

Takeaways:

* First he talks about why Nagios is good and why it's widely adopted
* But...
	* It doesn't scale (single centralized server that can't be properly clustered)
	* Horrible configuration (requires a config on *both* the client and server)
	* Horrible interface (no custom dashboards)
	* Assumes a static infrastructure (master must be told about a client before monitoring can begin)
	* No good programmatic interface (API)
	* Collects but throws away performance data
	* Stupid wire format for clients (NRPE/NSCA)
* **Sensu** is probably the best replacement
* Sensu is described as a "monitoring router"
* Uses a pub/sub methodology.  **RabbitMQ** sits between the server and the client so clients and servers never communicate directly
* To run a check, Sensu sends a message to the queue saying "Any production web server should run this check".  The proper clients then pick up the request, run the check, and return the results to RabbitMQ
* Sensu executes **handlers** based on the results of checks
* Use Graphite in your stack, it's the best graphing package
* Graphite supports anomaly detection.  This allows for proactive alerting when a metric is too high or too low for too long.  Better than a boolean working/broke check, much more flexible
* Alerting is not the concern of your monitoring tool
* You should push all of your alerts to **Flapjack** and create relationships between checks and gateways
* The default Sensu dashboard and sensu-server stinks, many missing features - but there's no good alternative right now
* There is a Nagios UI written in Minecraft!
* He feels that Sensu / Graphite / Flapjack community is much more responsive than the Nagios community

tl;dr Sensu + Graphite + Flapjack + Redis + RabbitMQ instead of Nagios