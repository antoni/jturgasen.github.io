---
layout: post
title: "Tech Takeaway 014: #ChefConf 2014: Chris Fortier & Manny Toledo: Moving Data Centers Is Scary"
description: ""
category: 
tags: ["chef", "tech takeaway"]
---
{% include JB/setup %}

* They moved from bare metal / colo Rackspace to AWS
* Don't do it all at once, do it in steps
* Move dev environments first, then QA
* Next, make all new features **start** in AWS
* They give each developer a "bodega", which is a VM that contains a superset of all of their applications.  An all-in-one dev system
* They provision the bodega with Chef.  This also serves as a great test: If they can provision the bodega, then they can likely provision the same apps to prod
* Don't make a cookbook that pulls down a bash script and runs the script
* Their workflow -> wget a bash script -> run script -> script sets up Vagrant -> Vagrant calls Chef
* Have infrastructure spin up when you need it, and blow it up when you don't
* Dogfooded the cookbooks by destroying and re-creating the bodegas every day
* Know your versions and dependencies.  Use dedicated repo mirrors to control this
* Repo mirrors are based on location.  A Rackspace node hits their Rackspace repo mirror, an AWS instance hits AWS repo mirror, etc
* The above idea saves both time (WAN vs LAN) and money (external network charges)
* They made each data center (Rackspace and AWS) it's own Chef environment.  This is how it knew which repo mirror to pull from, this is the source code repo to use, etc
* ``node ['be']['location']``  ``be`` is a top level attribute above every cookbook.  eg: ``be=rax be=aws``
* They used this to make simple if or case statements.  eg: php 4.5 in rax, php 4.4 in aws
* They use "gatekeeper attributes" to set defaults and then change from the default when necessary using simple if statements
* Review 20:00, they spell out a great abstration layer
* Peer review changes before you merge to master!
* Cookbooks are frozen and only Jenkins can push cookbooks to the server.  Jenkins (and it's tests) act as a gatekeeper.
* Jenkins does the tests on the cookbooks.  If they pass, Jenkins also does the ``knife upload`` to the server
* Test builds over slow links to find annoyances and inefficencies.  "Why am I downloading this 20m tgz every run?"
* Because they use Jenkins to test and push cookbooks they keep the test status for each cookbook on a dashboard on the wall
* They automatically build and rebuild a bodega every hour to make sure all steps in the flow work
* "Code wins arguments"
* Chef is idempotent, your cookbooks might not be
* Lock cookbooks to make sure everything works together.  "Lock environments"
* They don't let anyone make a change to any part of their infrastructure without a Git pull request.  "Human visibility"