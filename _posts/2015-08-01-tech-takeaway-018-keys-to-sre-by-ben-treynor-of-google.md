---
layout: post
title: "Tech Takeaway 018: Keys to SRE by Ben Treynor of Google"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[Keys to SRE by Ben Treynor of Google, presented at SREcon14](https://www.youtube.com/watch?v=n4Wf14e2jxQ)

Takeaways: 

* Why have a SRE team?  Because **reliability is the most important feature**
* What's more important feature in a product: That the product works, or the newest feature?
* Reliability is easy to take for granted.  Just because it has always worked doesn't mean it will always work.
* By the time things become unstable a number of things have failed
* Work on reliability all the time, not just when things are on fire
* It can take months or even years to make a system reliable after a failure so it's best to do the work up front
* Devs want to launch features and see them adopted.  Ops wants to make sure things don't blow up.
* The number one cause of outages and errors is when you change something
* Mitigate the risk of changes by...
	* Launch reviews.
	* Product deep-dives that include ops.
	* Release checklists
	* Extended canary process
* Consider where to place the canary (think about traffic, usage patterns)
* Consider putting the canary in each region
* **Beware** of statements from the developers like "this isn't a release, it's a feature add / flag flip" or "it's a low risk UI change"
* As organizations build process around their release process the devs will find ways to mitigate that process
* Devs know the most about the code.  The team that knows the least about the code (operations) has the strongest incentive to object to it's launch
* SRE does not attempt to assess launch risk, avoid all outages, or set release policy
* Google uses **error budgets** to determine launches
* For example, if you serve 1 billion queries per month and need 99.9%, that means 1 million queries per month can fail.  That's your budget.
* What do you want to spend your error budget on?  Launches or outages?
* Determine your SLA.  Not everything needs 99.999%
* When determining your SLA **think about the client device's reliability**.  If the client device is only 98% then your service can be 98% too.
* If the service is in SLA, launch away, devs are doing a good job
* If the service is not within SLA freeze the launch until you earn back enough error budget
* Error budgets make launch/no launch a math problem rather than an opinion or power conflict
* If you use error budgets the dev teams will quickly realize that they must self-police.  If they don't, they won't launch as often as they would like.
* You can throw people at badly functioning systems to keep it alive via manual labor but that sucks.  Ops ends up owning all of the crap work.
* Devs don't see the work that ops has to do so they may forget it exists or that ops is overburdened.  If I don't see it, it doesn't exist.
* At Google SRE and devs are a common staffing pool.  Managers must determine the ratios.  When more operations work is required (SRE) it means fewer software engineers for new features (dev)
* Google SRE only hires coders because they speak the same language as the devs
* They also look for people that get bored easily (I've done this 4 times, I'm bored with it, let's automate it)
* They cap the amount of ops work that a SRE does so that the SRE can write more code because coding reduces the work to traffic ratio
* Make sure to keep the devs in the SRE rotation.  "Nothing builds consensus on a bug priorities like a couple of sleepless nights in a row"
* Every dev team will resist this.  You need management buy-in.
* After about 6 months they won't want to go back to the old way
* Excess operations load always gets assigned to the dev team.  This creates a self-regulating system
* SRE teams can be disbanded if all of the SREs are having problems with the devs.  If it happens, the SREs are moved and the devs are now on-call
* This is incentive for devs to work with and listen to SRE because if they don't then they are the ones who get to deal with all of the ops problems
* Two goals for each outage: minimize impact and prevent recurrence
* There's **no NOC** because it's essentially just a load balancer and they add latency.  Send the alerts to a person who can actually fix the problem.
* You need good diagnostic information if you want high uptimes
* "There is a problem on this cluster" = bad error report
* "You are seeing high latency on X% of queries from this cluster to that cluster" = good error report
* Rather than using "operational readiness" drills to practice, they use **Wheel of Misfortune** (think Netflix Chaos Monkey)
* Write **post-mortems**.  You don't want the same outage twice.
* They have explicit limits on how many events per oncall shift a SRE can handle.  This is so SREs can do proper post-mortems
* Google uses **blameless post-mortems**.  They focus on process and technology, not people
* Post-mortems should have a timeline, they should have all the facts, and you should reate bugs for the followup work