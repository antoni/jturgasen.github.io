---
layout: post
title: "Tech Takeaway 024: 10+ Deploys Per Day: Dev and Ops Cooperation at Flickr by John Allspaw and Paul Hammond"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

["10+ Deploys Per Day: Dev and Ops Cooperation at Flickr by John Allspaw (Flickr/Yahoo!) and Paul Hammond (Flickr), presented at Velocity 09](https://www.youtube.com/watch?v=LdOe18KhtT4)

Takeaways:

* Ops job is to enable the business.  Keeping the site stable and fast is common among business requirements
* **Change** is the root cause of most outages
* So you can either discourage change or build tools and culture to allow change to happen as often as it needs to
* Automate infrastructure.  Saves manpower and creates a consistent, reliable platform for developers
* Use a single VCS for dev and ops to provide transparency
* Set up a **one step build process**.  No "run commands A B C".  Automate it.
* **Builds should be centralized** so differences in developer workstations don't impact builds
* Create a one step deploy process (eg: a single button, a make script, shell script, etc)
* Be sure you have a **deploy log**.  This can act as change management.  It tells the who, what, and when
* Consider adding a **"last deploy" panel** to your monitoring dashboard
* They use **feature flags** in the code instead of VCS branches
* Web apps use a different branch method than desktop apps.  With webapps all you have is whatever is currently deployed, no true branches for v1.0 v1.0.1 v2.0 etc etc
* By always shipping trunk it's easy for people to see what's currently deployed
* They talk about canary deploys but they call them "bucket deploys"
* **Dark launches** are when you turn on a feature but it's not displayed to the end user.  Everything happens as normal but the end-user doesn't see the feature
* Dark launches allow you to monitor the performance of the new feature (increased DB load, etc) without actually launching the feature
* Dark launches are a boon to ops because it takes the risk out of a feature launch
* Another advantage of feature flags is they act as **contingency switches**.  If feature X starts killing performance you can quickly switch it off
* They don't roll back often.  Instead, they **roll forward** and just turn things off via feature flags
* Feature flags **don't have to be binary - they can be knobs**
* Give devs access to dashboards / metrics too
* Be sure to **collect application-level metrics** too, not just CPU disk mem etc
* App level metrics allow you to create **adaptive feedback loops** for async operations
* Adaptive feedback loops allow you to dynamically adapt to load (eg: too many requests?  automatically throttle them)
* Include the time of last deploy on *all* metrics/monitoring pages.  Makes it easier to see if the deploy caused a problem or increased performance
* Talks about sending events to IRC / HipChat
* They save IRC logs in a search engine - neat idea
* Have a culture of respect and avoid stereotypes
* Respect different people's expertise, opinions, and responsibilities
* Don't just say "no", find out what problem they are trying to solve
* Devs should talk to ops about the impact of a code change
	* What metrics will change and how?
	* What are the risks
	* What are the signs that something has gone wrong?
	* What are the contingencies?
	* Devs must answer these questions *before* talking to ops.  Devs don't need all the answers but need at least some of them
* Ops need to talk to devs about infrastructure changes
* Trust that everyone is going to do their best for the business
* Create shared escalation plans and runbooks - not just for ops
* **Devs need to add knobs and levers that ops can change** (eg: don't hard code 20 worker threads, make it variable and allow ops to change it)
* Give devs some kind of access to the system (logs, core dumps, etc etc) so they don't have to go through ops every time.  Use read-only shell accounts for best results
* Don't spend all your time thinking about how to prevent failure, but also think about how to *respond* to failures.  Practice, consider fire drills
* **Involve junior members in fire drills** to train them
* Blameless culture is great because it also decreased time to recovery.  No spending time blaming, CYA, finding fault, etc
* Devs need to remember that your mistakes will wake other people up at night
* If you break something and wake someone up, tell them you're sorry