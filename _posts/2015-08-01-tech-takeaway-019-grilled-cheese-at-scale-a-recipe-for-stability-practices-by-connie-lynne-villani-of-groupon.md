---
layout: post
title: "Tech Takeaway 019: Grilled Cheese at Scale: A Recipe for Stability Practices by Connie Lynne Villani of Groupon"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[Grilled Cheese at Scale: A Recipe for Stability Practices by Connie-Lynne Villani of Groupon, presented at SREcon14](https://www.youtube.com/watch?v=bwxY2IBmDhc)

Takeaways:

* Early on you're in a reactive operations mode because your number one priority is to get the product to market
* In general you want to...
	* Anticipate demand
	* Determine resources
	* Develop supporting processes
	* Set expectations
	* Plan for mistakes
	* Celebrate success
* The above list applies to software companies and to making dinner for 5 people
* People are more likely to do what you want them to if you build consensus at the beginning
* "Everyone agrees that change needs to happen but nobody agrees what that change is"
* You can have autonomy but only after you build consensus and trust levels
* First approach to building consensus...
	* Start in small groups
	* Identify areas of conflict
	* Work out the details
	* Write it down
* The second approach to building consensus...
	* Gather a lot of opinions
	* Identify interesting ideas
	* Work out the details
	* Write it down
	* Send the ideas out, ask people to vote for their top five, and go from there.  This creates buy-in.
* The first is good for large groups and for meetings with many agenda items
* The second is good when you think everyone is in agreement but they aren't talking to each other
* **If you don't write it down you won't remember it**
* **If you don't write it down you'll probably have the same arguments again**
* They use a 0 to 5 scale to build consensus.  If most people are 4 or 5 they can move on.  If most people are 0 to 3 the topic needs more discussion.
* In order to anticipate demand you must know your customers.  Marketing probably knows your customers, but ops probably doesn't
* To determine resources ask...
	* What can your existing infrastructure handle?
	* Can you repurpose other resources and/or add new resources?
	* How do people access these resources?
	* What specialized knowledge do you need?
	* How do you transfer knowledge?
	* How do you assess knowledge?
* **Marketing / sales needs to communicate with ops** for those big traffic bursts.  If not, marketing will assume you can do the magic because you've always done the magic
* Really the above **applies to all teams** (marketing talking to DBAs, network guys talking to sales, etc)
* When developing supporting processes ask...
	* How do I get something into production?  Gated or permissive model?
	* What safety measures are in place?
	* How do I find out what's in production?
	* What happens when a problem is detected?
	* What do I do in an emergency?
* When writing document also include information about where you want to get to (eg: future goals) of the project
* **Keep marketing/sales in the loop when a problem is detected** or there's an emergency
* When setting expectations consider...
	* What tradeoffs happen?
	* What safety measure are in place?
	* What should customers expect to see?
	* Would this be better served with a staged rollout?
	* What happens when something goes wrong?
* Instead of calling them runbooks they call them "owners manuals".  It creates a sense of ownership
* Consensus drives stability
* You should enable autonomy because...
	* People are more invested when they are self-directed
	* People will drive their own excellence
	* Nothing rides on a single person or team's shoulders
* Plan for mistakes by asking...
	* Can you revert quickly?
	* What SPOFs do we have?
	* Can we degrade gracefully?
	* How do we learn from errors?
	* What happens when something goes wrong?
* If your release takes two hours your rollbacks will probably take two hours too
* Don't just fail fast, you must also **fail intelligently**
* When doing post-mortems also look at things you **did right** during the outage
* When a launch goes well, consider talking about what you did right and what you did to enable a smooth launch.  **Root-cause analysis on successes** too.
* They have a **panic button** for non-IT groups to contact IT if they see a problem (eg: lots of clients are calling sales because the web site seems broke)
* When people hit the panic button in an inappropriate way tell them about the resources they wasted by doing so