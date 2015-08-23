---
layout: post
title: "Tech Takeaway 029: Automating Infrastructure Management with Terraform by Armon Dadgar"
description: ""
category: 
tags: ["tech takeaway", "terraform", "cloud"]
---
{% include JB/setup %}

[Armon Dadgar of Hashicorp talks about Terraform at the SF CloudOps Meetup in October 2014](https://www.youtube.com/watch?v=WdV4eYZO5Ao)

Takeaways:

* To him, orchestration is **infrastructure lifecycle management** that consists of
	* Acquisition
	* Provisioning
	* Updating
	* Destroying
* Talks about the old way of doing things (weeks to get a server, manual provisioning/updating/destroying)
* Talks about how the cloud is "utility computing"
* You can aquire the infrastructure quickly with cloud (AWS via a credit card), but how do you provision quickly?
* Config management abstracts away complexity because you don't need to know the shell code to put a file into place with X mode and Y owner, you just need to know Chef/Puppet/etc
* Config management lets you "codify knowledge" by putting it into shared repos
* Automation is less error prone than manual processes
* Containers...
	* OS-agnostic packaging
	* Simplify delivery
	* Lets you use config management inside of it
* Talks about the rise of SaaS and how it allows companies to focus on their core compentency rather than manage an Exchange server
* SaaS is great, but there are so many SaaS providers that it introduces *more* complexity
* Operators / sysadmins
	* Understand infrastructure so you can fix it
	* Adopt new technology and stay nimble
	* Deploy efficiently - move fast without breaking things
* The goals of Terraform
	* One workflow that's technology agnostic
	* Focuses on the modern data center (public cloud + private cloud + SaaS + legacy)
	* Make PaaS and SaaS a first class entity because they aren't going away
	* Operator first
* He feels infrastructure as code is
	* Description of the desired state
	* Management automation
	* The infrastructure is documented via code
	* Knowlege sharing (eg: save Chef and Terraform info in git for all to see)
* Terraform must be flexible and support
	* The entire data center
	* PaaS and SaaS
	* Networking
	* Storage
* Terraform uses HCL (Hashicorp Config Language)
	* Based on libucl
	* Human readable
	* JSON interoperable
* Terraform uses **resources** like Chef and Puppet
	* The first part of a resource defines it's type (eg: digitalocean_droplet or dns_record)
	* The second part of a resource is it's logical name (eg: webserver, foodns)
	* The third part of a resource is the parameters (OS image, region, memory size, DNS record type, etc)
* Terraform does does dependency management for you (eg: does the DNS record come first or does the DO droplet come first)
* Terraform recognizes the ordering of operations by creating a **resource graph**
* The resource graph also lets you visualize operations and perform non-dependent operations in parallel
* **Providers** are
	* The integration points (eg: AWS, Azure, etc)
	* They expose resources
	* Provides a simple CRUD API that abstracts Terraform from the provider
* Talks about how infra is layered (physical -> virtual -> containers)
* It's easy to roll out infra, but it's hard to change it because you usually don't know what depends on what
* Terraform separates planning from execution
	* Operator determines what changes to make
	* Terraform creates an **execution plan** and shows it to the operator
	* If it looks good the operator tells Terraform to apply the plan
	* The flow is State + Configuration -> Execution Plan -> Plan Application -> State
* The tenants of Terraform are
	* Infra as code for understandability and documentation
	* Compose providers so you can express the layers of infra (physical virtual container)
	* Safely iterate (plan -> apply)
	* Technology agnostic
* Terraform **modules**
	* Abstract infra components
	* Provide "higher level reasoning" (think Chef for OS files)
	* You can re-use them (DRY)
* Talks about how operations are on the front line of enabling a CD model
* Talks about how self-service is the ultimate end-game
* In the Terraform pipline
	* Ops takes care of the modules and "substrate" (eg: lower level stuff, VPN, storage, etc)
	* Developers just plug-and-play
* Terraform enables the operators and by doing that it also enables Devs and end-users via self-service
* You can't rollback changes
* If a step fails subsequent steps won't be applied, but steps that are being performed in parallel will be applied
* If a plan+execution fails the next plan+execution will handle cleanup of partial changes
* Note how abstractions are per-provider.  They do not abstract to the level of "just a VM" but instead they use a "AWS VM"
* Many tools tried to abstract to the VM level, all failed, that is why they are not abstracting this far
* Execution plans tell you if it's going to do a create -> destroy operation
* Terraform maintains a state file.  You can run a command to refresh it and/or it refreshes each time a plan phase runs
* They are exporing using a centralized state repo like git instead of a file
* They are working on a feature that will read your AWS config and spits out Terraform templates
* If you tell Terraform to spin up 4 web servers you can flip a variable and change it to 5 - no need to create a new template
* Terraform is not an application scheduler.  It can create 4 Docker containers on a VM but you can't say "I want these 4 containers living on any VM"