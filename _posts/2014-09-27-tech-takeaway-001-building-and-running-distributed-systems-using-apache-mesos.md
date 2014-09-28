---
layout: post
title: "Tech Takeaway 001: Building and Running Distributed Systems using Apache Mesos"
description: ""
category: 
tags: ["mesos", "tech takeaway"]
---
{% include JB/setup %}

Most of Tech Takeaways will not be this large, I had no idea this one was going to be so in-depth!  

[Benjamin Hindman talks about Mesos at ApacheCon North America 2014](https://www.youtube.com/watch?v=hTcZGODnyf0)  

Takeaways:

* Read the paper [There's Just No Getting around It: You're Building a Distributed System](http://queue.acm.org/detail.cfm?id=2482856) by Mark Cavage
* In the Mesos vocabulary **frameworks** run on top of Mesos and act as coordinators
* Another term for a framework is **scheduler** and the workers are called **tasks** or **executors**
* Mesos sits between the frameworks (schedulers) and the slaves/workers and provides an abstraction layer so the frameworks don't have to talk directly to the machines
* Frameworks don't need to worry about failure detection, task start/stop/cleanup/killing/monitoring, and task distribution - Mesos does that instead
* Because Mesos acts as an abstraction layer we can have multiple frameworks talking to Mesos and they all share the same pool of resources
* Mesos manages *resources*, IaaS sits below it and manages *machines*, and PaaS sits above it and manages *applications and services*
* Resources are essentially the parameters of the task, what it needs.  For example:
	* I need my task to run on 8 different physical servers
	* The task needs to be fault tolerant so I need each physical server to be in a unique cluster
	* Each task on a system needs 2 cores
	* Each task on a system needs 8 gigs of RAM
* **Calls** are messages sent from the scheduler/framework to Mesos (eg: run this task)
* **Events** are messages sent from Mesos to the scheduler/framework (eg: task ended, here's the results or task died, what should we do?)
* Uses the call/event message passing model instead of the request/response model because some events are generated without a request
* Events are primitives, I'm not sure if calls are too
* **Calls** are essentially broken down into 3 types: lifecycle management, resource allocation, and task management
	* The lifecycle management primitive REGISTER is used to register a framework with Mesos
	* The lifecycle management primitive REREGISTER is used to reregister a framework with Mesos
	* The lifecycle management primitive UNREGISTER is used to unregister a framework with Mesos
	* The resource allocation primitive REQUEST requests resources
	* The resource allocation primitive DECLINE declines a resource offer from Mesos (eg: not guaranteed enough RAM for the task)
	* The resource allocation primitive REVIVE restarts a task
	* The task management primitive LAUNCH is used to launch a task
	* The task management primitive KILL is used to kill a task
	* The task management primitive ACKNOWLEDGE is to acknowledge the status of a task
	* The task management primitive RECONCILE is used to sync state (what you think the task should be doing vs what it is actually doing)
* **Events** are broken down into the same 3 types: lifecycle management, resource allocation, and task management
	* The lifecycle management primitive REGISTERED tells the scheduler it has been successfully registered / authenticated
	* The lifecycle management primitive REREGISTERED is used when a scheduler faults and is restarted
	* The resource allocation primitive OFFERS sends resource offers to the scheduler, probably after a REQUEST
	* The resource allocation primitive RESCIND rescinds resource offers
	* The task management primitive UPDATE sends updates from a task to the scheduler (eg: current state of the task)
* The flow of a scheduler/framework running a task:
	* Scheduler sends a REGISTER call to Mesos.  Note that authentication can be done at this point.
	* Mesos responds with REGISTERED event
	* Optionally, the scheduler can send a REQUEST call.  **NOTE** This acts as a "hint" for Mesos event OFFERS.  OFFERS can be sent before a REQUEST is sent.
	* Mesos starts sending OFFER events to the scheduler/framework
	* The scheduler uses OFFER events to determine which task(s) it wants to run
	* The scheduler then sends a LAUNCH call to Mesos
	* Mesos then starts the task on the specified slave(s)
	* While the task runs Mesos continues to send OFFER events to the scheduler in case the framework has more stuff it needs to run
	* Later, Mesos will send an UPDATE event to the framework/scheduler (task complete, task failed, task lost due to hardware failure, task running, etc)
* Currently officially supports C++, Java, and Python API bindings
* These are the "medium level APIs" while the primitives (OFFER, LAUNCH, REGISTER, etc) are the "low level APIs"
* **SchedulerDriver** is used to make calls from the scheduler/framework to Mesos
* In the SchedulerDriver the calls to Mesos are implemented as methods
* The scheduler/framework talks to the SchedulerDriver which in turn talks to Mesos
* SchedulerDriver is responsible for finding Mesos (eg: failover scenario, service registry, etc)
* The SchedulerDriver passes low-level events from Mesos to the scheduler/framework and sends medium-level calls from the scheduler/framework to Mesos
* The SchedulerDriver is linked against **libmesos**
* libmesos uses HTTP to talk to Mesos so they are trying to remove it
* 99% of the types you will use with Mesos are protocol buffers
* The components of a REGISTER call:
	* **user** is the user name that your tasks run as, can be root if necessary
	* All tasks from a framework run as the user.  They are working to change this so tasks can run as multiple users
	* **name** is the human-readable identifier for your framework
	* **FrameworkID** is the machine-readable identifier for your framework
	* Frameworks/schedulers can fail.  They can then restart on another node and connect to Mesos using the same FrameworkID and communications resume
	* Instead of a straight up "failure" scenario you might use this to upgrade/patch a scheduler without stopping all tasks
	* **failover_timeout** tells Mesos how to react when the framework fails
	* If the failover_timeout is set to 0 it means Mesos should kill all tasks, this is the default
	* If failover_timeout is set to a number then it's the number of seconds that a Mesos should wait for before killing all of the framework's tasks
	* In practice, most people either want to kill all tasks when a framework fails or to keep all tasks running when a framework fails (eg: 0 or infinite)
	* **checkpoint** can either be false (the default) or a number
	* Checkpoint tells Mesos how to react when the mesos-slave process on a worker node dies (kill them or let them run for X I think)
	* **role** is the organization or group that your framework's task(s) should take resources from
	* Can be a number or an asterisk.  Asterisk = any
	* Role allows you to split up the resources by organization/group, fair sharing, resource reservations, etc
	* **hostname** is the name of the actual host that the framework is running on
* The components of an OFFER event
	* **OfferID** is the machine-readable identifier for the offer
	* **FrameworkID** is the same as the one explained above in the REGISTER section
	* **SlaveID** is the machine-readable identifier of the slave (VM, bare metal host) that the offer is from
	* **hostname** is unknown, he skipped it!  Probably the same as the REGISTER hostname
	* **resources** are what's available on the slave to run tasks (amount of CPU, memory, disks)
	* **attributes** is unknown, he skipped it!
	* **executor_ids** is unknown, he skipped it too!
* The components of a LAUNCH call (to start a task):
	* **name** is the human-readable identifer
	* **TaskID** is the machine-readable identifier of your task, like a PID
	* **Resource** is the resources your task is using, might be less than the OFFER
	* **ExecutorInfo** is 
	* **CommandInfo**
	* **bytes** 
* TaskInfo is what describes the task, lives within a LAUNCH call
* An **executor** sits between the mesos-slave process and the task on a node
	* Creates a level of "indirection" or abstraction.  Allows a single executor to run multiple tasks.  Tasks can be shell commands, threads, whatever
	* Containerization is either around a single task or around an executor and it's tasks
	* When an executor is containerized you can scale the container based on how many tasks the executor runs
	* I assume the executor's container has a hard limit on resources but can scale down when appropriate
	* Using the executor API is optional
	* The executor sends **calls** to the mesos-slave process and mesos-slave sends **events** to the executor
	* Similar to the Mesos and framework/scheduler interaction, executors have REGISTER, REREGISTER, UNREGISTER, UPDATE, LAUNCH, KILL, etc
	* By sending "generic" commands to the executor we allow it to realize the execution of the task however it wants (threads, shell commands, python, etc)
* The Mesos CLI tool has a syntax that's similar to git
* Web UI is a REST interface
* **Command Executor** sits between the task and the mesos-slave.  Can kill the task when the slave process dies
* Zookeeper is only used for **leader election** of a master
* They are working on removing the Zookeeper dependency
* Other masters are passive standbys