---
layout: post
title: "Why Use Configuration Management Software?"
description: ""
category: 
tags: ["chef", "puppet", "salt", "ansible", "configuration management"]
---
{% include JB/setup %}

Why should organizations use configuration management (CM) software like Chef, Puppet, Saltstack, or Ansible?

* **Minimize Human Effort** - CM software makes changes faster (reduces human effort) and more consistent (if you manually make a change on 20 systems you might make a typo on 1 of them).
* **Compliance** - You can use configuration management software to create stateful systems that comply with SARBOX, HIPAA, PCI DSS, or any other standard
* **Auditing** - No more trying to figure out who made a change to the server.  Look at the source code repository and/or CM logs instead of manually digging through multiple logs.
* **State Drift** - You need to make a change to 20 servers but one of them is down or unavailable.  Are you certain you'll remember to change it later?  Do you need to create a new change request and schedule a new change to fix that one server?  Let CM software automatically take care of it when the server becomes available.
* **Easier Post-install** - CM software simplifies the post-install process.  Rather than a mess of KickStart post-configs or custom shell scripts you can simply call your CM software and let it do the work.
* **Automated CMDB** - Let your CM software double as your CMDB.
* **Reporting** - Instead of writing a script, SSH'ing to a bunch of systems, gathering, and parsing the results, let your CM software do the work.
* **Automated Documentation** - Minimize or eliminate the amount of documentation you have to do.  Need to know how or why something was done a certain way?  Look at the CM code and it's comments.
* **Easier Rollbacks** - Need to rollback a change?  Just undo your changes in the CM software and you're done - no need for lengthy backout procedures when submitting a change request.
* **Faster Disaster Recovery** - Remove or minimize the amount of data you need to restore in a DR scenario.
* **Simplified Disaster Recovery** - Deploy your standard build, kick off the CM software, re-deploy (or restore) your code, and you're done.  Easy easy.
* **Abstraction** - Remembering the nuances of each OS can be daunting.  CM software abstracts it so all you need to do is say "install this package" instead of remembering the package install command for each platform.
* **Centralized Access Control** - No need to create and manage a bunch of accounts on each server (or in LDAP).  Because the CM software does all of the work admins will rarely have to login directly.

As you can see there are *many* advantages to using configuration management software.  It might even be worth the effort for your little 4 node home lab!