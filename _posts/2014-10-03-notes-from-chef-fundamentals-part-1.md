---
layout: post
title: "Notes from Chef Fundamentals Part 1"
description: ""
category: 
tags: ["chef"]
---
{% include JB/setup %}

Notes from the [Chef Fundamentals](http://learn.getchef.com/fundamentals-series) series of webcasts.

* **Resources** are an element of an OS that can be manipulated via code to achieve the desired state.  Examples include:
	* Networking
	* Files
	* Directories
	* Symlinks
	* Mounts
	* Users
	* Groups
	* Packages
	* Services
	* File Systems
* Store your code in a version control system!
* Add automated tests to your code!
* You can rebuild your business with your backups, code repository, and code resources
* Chef ensures each **node** complies with policy
* **Policy** is determined by the configurations in each node's **run list**
* Policy states what state each resource should be in, but not how to get there (eg: abstraction)
* **chef-client** will pull the policy from the server and enforce the policy on the node
* Resources are gathered into **recipes** and recipes are what ensure system state
* **Templates** support variables so you can, for example, put a FQDN variable into a httpd.conf so it's unique on each system
* An alternative name for a desired state is an **action** eg: httpd start
* You can specify multiple states/actions for a single resource eg: httpd start, enabled
* When the chef-client runs you could call it **test and repair** mode
* Templates (such as httpd.conf) can have attributes such as...
	* Source
	* Owner
	* Group
	* Mode
	* Variables within the file (eg: httpd.conf AllowOverride = All)
	* Notifies (eg: bounce httpd when it gets a new httpd.conf)
* A node's **run list** specifies the subset of policies that should be applied to that node (note subset, you don't want to apply ALL site-wide policies to a node!)
* Run lists are run in order
* Chef search allows you to search by role (web server, DB server, prod DB server, etc)
* Search can also help the Chef client to map out the topology so it knows what to install and where to install it (eg: DB/memcache/app/web tier)
* Supports **variables**.  You can set one, put it into a template or file, and Chef will then expand it
* To roll out Chef you should...
	* Determine the desired state of your infrastructure
	* Identify the resources required to meet that that
	* Gather the resources into recipes
	* Compose the run list
	* Apply the run list to each node
	* Done!
* **Configuration drift**...
	* Infra requirements change
	* The configuration of a server falls out of policy (eg: manual changes outside of Chef)
* Key terms:
	* Resource
	* Recipe
	* Node
	* Run List
	* Search
* A typical environment is made up of...
	* Chef server
	* Nodes (clients)
	* Administrator's workstation
* The Chef server stores cookbooks and **node objects**
* The admin workstation is where you author the cookbooks
* You use **knife** on the workstation to interface with the server.  Knife can also interface with clients/nodes
* **Hosted Chef** is hosted by Opscode, used if you don't want to have an on-premise server
* An **organization**...
	* Provides multi-tenancy in Enterprise Chef
	* Nothing is shared between organizations
	* May represent companies, business units, and/or departments
	* You could also have a single organization that represents your entire company
* **knife client list** to see a list of clients/nodes (I think?)
* knife supports **-VV** for verbose output / debugging

Video #1 complete, on to video two!