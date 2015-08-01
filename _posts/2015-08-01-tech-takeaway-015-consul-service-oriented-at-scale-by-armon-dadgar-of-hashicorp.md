---
layout: post
title: "Tech Takeaway 015: Consul: Service Oriented at Scale by Armon Dadgar of Hashicorp"
description: ""
category: 
tags: ["tech takeaway", "consul", "cloud", "service discovery", "microservices"]
---
{% include JB/setup %}

[Consul: Service Oriented at Scale, by Armon Dadgar of Hashicorp, presented at RICON 2014](https://www.youtube.com/watch?v=eR0899h_1Ac)

Takeaways:

* SOA is a spectrum, not a binary decision.  It slides between microservice and monolith models
* If you use only microservices you introduce a lot of overhead, both conceptual (100s or 1000s of services) and technical (network traffic / tons of API calls)
* The downside of monolith is the risk of introducing change to this single, large service
* SOA should be autonomous, the scope of each service is limited and well-defined, and the services are loosely-coupled
* SOA is great because you can...
	* Develop faster (iterate faster)
	* Composable
	* Failures are isolated
	* Each service is testable (well known inputs and outputs)
	* You scale out instead of up
* SOA is hard because...
	* Service discovery (where do I find the billing service?)
	* Monitoring (how do you monitor a very dynamic environment?)
	* Configuration
	* Orchestration
* Consul seeks to weave these all together rather than using multiple tools / silos for each function
* How does a node or service find other services?  You can hard code, but that's silly (inflexible, fragile, not scalable)
* You could hard code and point to a load balancer, but now you have a SPOF
* How do you orchestrate?  When you change something how do you get the message to each tier and how do you make them work together to implement the change
* He feels that most service discovery solutions (DNS, ZooKeeper, RMDBS) force you to "roll your own"
* Existing monitoring is a "knowledge silo" in that it doesn't feed information back into service discovery (eg: server died so it should be removed from the list)
* Consul's service discovery is broken down into...
	* Nodes
	* Services
	* Checks (Nagios compatible)
	* Uses JSON for registration (name, tab, port, health check script, script interval)
	* Has DNS and HTTP interfaces for service discovery
	* Has a HTTP API
* Service lookup via DNS uses the **.service.consul** TLD   (eg: dig slave.redis.service.consul)
* Notice that when you use DNS for service discovery the TTL is set to zero which forces load balancing by discouraging client DNS caching
* DNS service discovery uses SRV records so it can include the port number
* Discovery via HTTP API is much more feature-rich than DNS discovery
* Consul clients run their checks and **push** info back to the servers (vs servers SSH'ing to clients, gathering info, and pulling it back)
* The monitoring component only pushes updates when the status changes as opposed to pushing status at every monitoring interval
* The downside of this type of monitoring is because status isn't sent at every heartbeat you're not sure if the agent is stable or dead
* Consul's gossip-based failure detection is more scalable.  No single server or SPOF, no "info pull" every X seconds like normal monitoring
* Supports "long-polling" (eg: notify me the moment my configuration changes from now until eternity)
* **envconsul** allows you to set environment variables by pulling value(s) from keys in Consul's key/value store
* **consul-template** is a generic templating tool that creates templates dynamically based on the key/value
* He gives an example of using consul-template to dynamically update HAproxy as nodes register with the service discovery component and remove nodes when the monitoring component sees they have failed
* Consul's orchestration component consists of...
	* Watchers
	* Event System
	* Remote Execution
	* Distributed Locking
* Watchers watch certain components (key/value, health checks, etc) and kicks off a custom handler when it changes
* The Event System is essentially name + payload.  The Watchers then receive and handle the event
* Event System example: Flush memcached when event X is received
* Remote Execution is built on top of the Event System and is essentially parallel SSH that can filter based on service/nodes/tags ``consul exec -service service-name command``
* The k/v store supports client-side distributed locking.  The client can specify the consistency level of their locks, similar to Cassandra
* He considers Consult to be the "data center control plane" because it includes configuration, orchestration, monitoring, and service discovery all in a single tool
* Consul scales to thousands of machines and supports multiple data centers
* Each machine has a Consul agent
* Agents can be in either server or client mode
* Usually you have 3 or 5 servers per data center (odd number for quorum)
* Server agents will internally elect a leader.  The leader replicates data to the other server agents in the data center
* Client and server agents in the same data center talk using **LAN Gossip**
* Intra-data center server agent communication is via **WAN Gossip**
* Client queries can be answered by leader nodes (most consistent, slowest, SPOF) and/or non-leader server nodes (faster, distributed, but less consistent)
* Server agents transparently forward client requests if necessary (eg: client in DC1 wants to see a list of all Redis nodes in DC2)
* Clients never talk directly to server agents.  They talk to the local agent and the local agent then forwards the request.  This means the client never has to know the name/IP/location of the server
* LAN and WAN gossip pools use the **Serf** tool
* Serf is what actually provides the...
	* Gossip pool membership
	* Event system
	* Failure detection
* Nodes join a gossip pool by talking to **any peer** (as opposed to a hard coded IP address)
* Gossip layer supports protocol versioning
* When a node cleanly leaves the gossip pool Consul will automatically unregisters the nodes services
* Gossip events use a P2P method for delivery.  This allows for sub-second delivery on thousands of nodes
* Because every node as a Consul agent, developers can assume it's installed, depend on it, and use it (much like syslog)
* Internally they use consul-template and envconsul
* Consul includes a web UI
* Deploys are "event driven".  When new binaries are available a handler downloads them and then later, another event + handler does the rolling restarts
* Supports **client-side leader election**.  This allows you to deploy N+1 instances of a service and the agents can determine which instance is active and which instance(s) are backups
* When uploading large binaries their VagrantCloud service uses a **write-ahead log** so it can track the chunks that are being uploaded.  This allows for more consistency and graceful recovery from failures
* The write-ahead log is essentially a list of chunks that the client will upload in order to make the full BLOB
* One company is migrating from Puppet to Consul k/v to save time on converges
* Some unnamed companies are testing it at 20k and 100k node scale
* Allows for **reactive infrastructure** (neat term!)
* Supports TLS and has a token based ACL system (similar to Amazon IAM)
* The default behavior is to return services in the local data center.  Clients can ask for a different data center (eg: redis.service.east1.service.consul)
* They do their rolling restarts by picking a random sleep value (0 to 30 seconds).  An alternative is a semaphore with locks.
* Supports **safe locks**.  These are locks that are not released until the client explicitly releases it.
* Also supports **live locks**.  Live locks are released if Consul detects that the client has failed