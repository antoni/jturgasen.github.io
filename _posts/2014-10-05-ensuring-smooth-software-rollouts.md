---
layout: post
title: "Ensuring Smooth Software Rollouts"
description: ""
category: 
tags: ["best practices"]
---
{% include JB/setup %}

At a recent interview I was asked the following question: *How do you ensure smooth software rollouts and upgrades?*

I've been thinking about this question for a few days and here's what I brainstormed up:

* **Functionality Testing in the Lab** - Does all the stuff that worked before still work?  Do new features work as intended?
* **Load Testing in the Lab** - Does the new version still handle the intended load?
* **Backout Plan** - If the rollout fails what is your plan to rollback to the old version?
* **Bug Trackers and Mailing Lists** - If you're using an OSS product check the bug trackers and mailing lists before you push to prod
* **Vendor Errata** - If you're using a commercial software product check for late breaking information about known bugs before you push to prod
* **Point Releases** - Have known bugs been fixed via point releases? (eg: you plan to upgrade to 2.0, vendor finds a bug and releases 2.0.1)
* **Watch the Ticket Queues** - Is there a spike in incidents related to the software after the rollout?  If so, do they follow a pattern?
* **Check the Log Files** - Any new messages in the log files?
* **Compare Current to Historical Performance** - Did the app use 20% of the CPU before the upgrade but now it's using 80%?  What about RAM, network, and disk?
* **Put the Vendor on Alert** - If you're a big shop working with a small vendor make sure they know you're doing a major rollout/upgrade so they have people available
* **Put the Developers on Alert** - If you're rolling out an in-house application be sure the developers will be available during the rollout
* **Are Labs the Same as Production** - Maybe the lab boxes have beefier hardware specs, different OS versions, different package versions, etc
* **Rollout Slowly** - Upgrade 1 server, then monitor it.  Then 10.  Then 100.  Then 10% of your servers.  Then 25%.  Then 50%, and finally 100% of your servers.
* **Assess Disk Space** - Do you have enough unused disk space to do the upgrade/rollout?
* **Tested Backups** - If everything blows up can you restore from backup?
* **User Testing** - Round up some end-users and have them try the new version before your rollout.  A fresh perspective can give new ideas.
* **Read the Release Notes** - For both commercial and OSS software
* **Credentials** - Do the people doing the rollout have the access they need (root, console access, etc)?
* **Document the Process** - Write down the steps!
* **Automate It** - Reduce human error.
* **Tell the Help Desk** - Let them know so they can work with you to watch for problems
* **Tell End-Users** - Communicate the change so they know what's going on

What other ideas do you have?  E-mail me!