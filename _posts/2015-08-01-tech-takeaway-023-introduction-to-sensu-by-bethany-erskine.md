---
layout: post
title: "Tech Takeaway 023: Introduction to Sensu by Bethany Erskine"
description: ""
category: 
tags: ["tech takeaway", "monitoring"]
---
{% include JB/setup %}

[Introduction to Sensu by Bethany Erskine at nycdevops 2013](https://www.youtube.com/watch?v=30JwqsNi1GA)

Takeaways:

* Why Sensu?
	* Written in Ruby
	* Plugins can be written in any language
	* The sensu-chef cookbook is great
	* Solid community
	* Can re-use Nagios checks
	* Graphite integration
	* Easy to scale
* Sensu is sometimes called a **monitoring router**
* 4g RAM + 4 CPU is enough for a single VM to run RabbitMQ + Redis + Sensu
* 1.5g RAM + 2 CPU is enough for a Sensu server
* They started with a single server and knew it was time to scale when the server's load got insane and the RabbitMQ queues started to back up
* Monitor the RabbitMQ **ready queue size**.  If it hits about 10k and stays there, send an alert
* Checks are distributed among Sensu servers using round-robin - add more servers and go
* Graphite has been their main pain point
* They send metrics from the Sensu server to a special message queue and Graphite consumes that queue via AMQP, but Graphite sometimes can't keep up
* To stop Graphite from getting overloaded they changed from 10 second intervals to a larger number, made staging it's own Graphite cluster, and moved the prod Graphite cluster to SSDs
* Instead of porting checks from Nagios they "nuked and paved"
* Nuke and pave is also a great opportunity to re-evaluate your checks because you probably have lots of non-actionable alerts
* Rather than alerting for high memory usage they changed it to be gathered as a metric instead
* Gather both system *and* service metrics
* Plan out your new monitoring system.  Plan out which checks run on which types of servers
* Checks must be actionable!
* Plan out your handlers.  Smaller things can be e-mail but super important things should be sent via PagerDuty
* Check for disk usage, swap usage, zombie processes, read-only file systems
* Collect metrics from vmstat, disk usage, CPU, memory, interface, and disk perf
* They use e-mail, PagerDuty, and Campfile as handlers
* After you define your generic alerts and metrics, determine which role-specific checks and metrics you need (web servers, DB, etc)
* If she could do it over she would create a custom Sensu cookbook and wrap the sensu-chef cookbook
* Create a crash and burn server for Sensu testing.  Great for testing Sensu upgrades, configuration changes, and testing your CM tool (Chef etc)
* Create a workflow for implementing, testing, deploying, and signing off on checks
* Get developers involved.  Let them add checks and metric collections
* They put their application checks within the application's cookbook (eg: Nginx checks are in the Nginx cookbook).  Neat, but not good if you change to another monitoring system in the future
* Sensu community plugins should be your first choice followed by Nagios plugins followed by custom code
* Set up 3rd party monitoring for the Sensu servers.  Who watches the watchers?  Monitor your monitoring.
* They use the 3rd party monitoring to make sure the server is alive and the disks aren't full - nothing exciting
* Have your test/dev/stage Sensu environment monitor your prod Sensu environment
* Sensu can collect metrics about itself
* After you deploy to prod you will likely need to tune your alerts
* RabbitMQ sits between the clients and the servers.  Clients and servers do not communicate directly
* Redis is used to persist data that is used by the API
* Sensu has an omnibus installer (Sensu + Rabbit + Redis + Ruby)
* They see the most load from metric collections and not from alerts
* The servers and clients use a JSON config file that configures Rabbit + Redis + API + Dashboard
* There are two ways to implement checks...
	* **Stand-alone checks** are defined and scheduled on the client itself
	* **Pub/Sub** are when the server tells the clients which checks to run and when to run them
* Pub/Sub checks are also known as **subscription checks**
* In the subscription model the server(s) send messages to RabbitMQ that say something like "all prod web servers check X".  The clients listen to the queue, act, return the results to RabbitMQ, and the server(s) pick up the results
* Handlers takes action on an event using TCP or UDP or pipe or AMQP or other handlers
* Handers are how you "route" alerts.  For example, you can route non-critical alerts to a HipChat room and send critical alerts to PagerDuty
* You can create custom handlers to remediate alerts (eg: auto-bounce a process when the process stops responding)
* Make sure the proper Ruby Gems are installed for your handlers.  Don't forget that Ruby is included in the omnibus distribution
* A check sends a **result** as an **event** to a handler
* You can specify multiple handlers for each check
* Has a REST API.  You can ``curl -l http://hostname:4567/events`` on the Sensu server or /info for other stuff etc etc
* The first time the client starts it registers with the server
* If the client/server keepalive fails the server assumes the client is down
* Can send events directly to Sensu via ``nc`` by sending JSON
* **Aggregate alerts** are useful for preventing alert floods
* For example, alert when X amount of checks are *not* ok
* RabbitMQ has a nice web interface, use it
* Lock your plugins Gem dependency versions for plugins
* If you write custom Sensu-related tools have them make use of the API
* If you use sensu-dashboard put Nginx in front of it because it runs on a weird port and makes you login each time.  Note that this is convenient but insecure
* They have a plugin that polls Graphite's API and sends alerts based on the output
* Mutators are handler-specific data massagers that can alter event data before it is passed to a handler.