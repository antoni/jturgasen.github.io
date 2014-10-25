---
layout: post
title: "Tech Takeaway 009: How to Fuck Up Your Configuration Management Adoption and Piss Everyone Off by Sascha Bates"
description: ""
category: 
tags: ["tech takeaway", "chef"]
---
{% include JB/setup %}

[Sascha Bates shows us how to not fuck up our configuration management rollout](https://www.youtube.com/watch?v=pHmU0aNkENc&list=UUxEieNpB_tXiUBoF9zkPmAw&index=45)

* Understand the different types of technical debt
* Acceptable technical debt = ugly, but it works and won't kill us later
* Unacceptable technical debt = it will hurt us later (lots of pain, time, and money)
* You need packages, you need to keep them in a repository.  No free range packages.
* Dev: "Oh hey I need this thing, here's a tarball I downloaded from the Internet."  No.
* Don't check big binaries in to your source control system, use real packages
* Package doesn't work in dev.  You fix it but don't update the package/install scripts.  Still doesn't work in QA when someone else deploys so now they get to re-figure it out.  Fix that shit up front.
* Pets vs cattle: Don't let dev become pets!  They tend to drift and get "special"
* "dev is production for everyone but the people who are buying stuff (eg: real customers like grandma)".  Breaking dev is not acceptable.
* When writing stuff in/for Chef you MUST do this at a minimum:
	* Syntax test with Knife
	* Line with Foodcritic
	* Vagrant for basic local testing
	* Converge your Vagrant VM until it works (converge, fix bugs, repeat until 100%)
	* After you fix all of the error you do it 2-3 more times to make 110% it works
* Write functional tests with the Minitest Chef Handler.  Have them run after the runlist to make SURE things work as expected.  eg: Chef thinks service started but it really didn't.
* Test Driven Deplyment (TDD)
* Write code, not scripts
* Don't port old scripts into Chef, you will pay the price later
* By importing scripts you lose idempotency
* DSL trumps code.  Using pure Ruby makes shit harder for the next guy.
* With functional testing it should test to make sure the things you set up are doing what they are supposed to do
* Use a Ruby Gems mirror / repo locally