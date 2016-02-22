---
layout: post
title: "Tech Takeaway 035: Growing from the Few to the Many: Scaling The Operations Organization at Facebook by Pedro Canahuati"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[Growing from the Few to the Many: Scaling the Operations Organization at Facebook by Pedro Canahuati of Facebook at QCon 2013](http://www.infoq.com/presentations/scaling-operations-facebook)

Takeaways:

* Ops gets few rewards but many reprimands when the infra fails
* Ops doesn't own their own destiny - it's someone else's tools/infra/apps
* When he joined, FB's SREs were getting too much interrupt driven work
* ALL WE WANT IS PEOPLE WHO ARE HIGHLY EXPERIENCED SOFTWARE ENGINEERS THAT KNOW A LOT ABOUT DISTRIBUTED SYSTEMS DESIGN AND NETWORKING.  IS THAT TOO MUCH TO ASK?
* Yes.  They were dumbfucks who were looking for unicorns and when they found one they got to tell him 85% of his job is firefighting hahahahaa
* They also did not have a consistent way to evaluate candidate's skills
* Talks about huge growth and their inability to add new clusters quickly
* Talks about how tribal knowledge is bad
* **He realized that ops inablity to roll out clusters quick enough would keep the company from growing**
* They talked to the other business units, found out what they need, and then took that info to rebuild ops
* The "reset" also allows them to set new expectations
* Split off the cluster build people into their own team so they could focus on optimizing the rollout procedure
* In order to re-proirotize they
	* Found out what people were doing (specific actions/tasks)
	* Try to automate that
	* They would also pull metrics from things like ack'ed alerts, when people SSH into a box, etc
	* This allows them to quantify what's taking time
	* They revisit this every two weeks
* Conversations about moving an ops guy know knows a lot about app X to app Y for the long-term good of the company were hard
* They use server/rack/cluster/data center visualizations to show what's working and what's broke
	* Red square = broke server
	* Green = ok
	* Not just hardware-level monitoring!
	* Makes it easier to see patterns (eg: a whole rack is down)
* Ops Engineers go through the FB Software Bootcamp along with the devs
* Change the name of the ops team to symbolize changing
* Blameless post-mortems, etc
* A culture where failure is ok, etc
* Talks about different types of engineering culture
	* Collaborative
	* Cultivation
	* Competence
	* Control
* Grow people internally and/or take risks on junior people
* Scale interview questions with experience
* Hire for culture.  Understand what you have, what you want, and then talk about change
* To change the culture
	* Describe the future
	* Set new trends
	* Own your destiny (ops should build their own tools)
* If you were strong on 3 out of the 4 dimensions (networking, systems, programming, and ??) and compentent in the 4th they will train you
* They would then pair you with a person who is strong in your weak area
* Find ways to say "yes" to requests
* Make the SWEs think more like ops so they consider ops'ish things in their design decisions
* SWEs are on-call - they own the production apps
* Talks about feature flags and activating them based on age, demographic, etc etc
* Infra changes are logged, but are not "gated" so people don't have to wait on approval
* Instrumentation: Measure everything
* Ops fixing reoccuring problems is a crutch for SWEs.  If ops fixes your shit, what incentive does the SWE have to fix it?
* Automation....
	* Large scale systems require sophisticated software, KISS the software
	* Design systems with failure in mind
	* Automate carefully - too much can cause cascading failures
* FBAR = FaceBook Automated Remediation
* Most FBAR automation works because the humans that wrote it understand the interactions between systems
* eg: Bouncing a process is easy, but you should probably take it out of the load balancer first, etc
* Commonize infrastructure
* They built tools to migrate existing apps to the new common infra
* Off the shelf HW is stale and vendors move slow - DIY it (OpenCompute, etc)