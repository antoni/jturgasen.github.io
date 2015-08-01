---
layout: post
title: "Tech Takeaway 025: From Zero to Hero: Recommended Practices for Training your Ever Evolving SRE Teams by Andrew Widdowson of Google"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[From Zero to Hero: Recommended Practices for Training your Ever-Evolving SRE Teams by Andrew Widdowson of Google, presented at SREcon15](https://www.youtube.com/watch?v=pE7RLRea0MM)

Takeaways:

* Don't just throw people in the deep end (eg: putting them on-call ASAP, telling them go grab random stuff out of the ticket queue, etc)
* Put trust in new SREs.  This helps avoid imposter syndrome
* Build their confidence via training so the first on-call rotation isn't so scary
* Creating a formal training process reduced the time to first on-call from 9 months to 3 months
* SRE is learned "on the job"
* Keep existing SREs sharp.  If a SRE is on-call for a stack that never changes (thus never/rarely breaks) they won't learn much
* Existing SREs might know everything about their system today, but the system will change with time.  Keep up to date
* Newbie SREs need to "go broad quickly" because there are many individual systems that interact with each other
* Create a concrete, sequential learning experience where topics are presented in a logical, orderly manner as opposed to "throwing them in the deep end" where they learn randomly
* Telling the newbie to jump into the ticket queue is unfair because they have no idea about how systems interact, what each system does, etc.  The learning process is slow and random
* Senior SREs should give newbies a "tour of the system" and show them how systems interact, how data/requests flow, etc
* Seniors should suggest which system(s) newbies should dig into first
* Consider designing **learning paths** (newbies should learn X then Y then Z)
* Create excellent reverse engineers so they can adapt to new systems
* Use organization-wide design patterns, frameworks, etc so newbies can be brought up to speed faster and can re-use knowledge
* Good reverse engineers can quickly "prune the decision tree" to determine the most likely causes of an outage
* Write post-mortems for each outage and keep them in a centralized, public area so newbies can learn from them
* Or, create a **curated list of post-mortems** that newbies should read and learn from
* Create a **post-mortem reading club**, similar to a book club, to read and discuss post-mortems as a group
* Create **disaster role playing** scenarios so SREs can practice together.  A "game master" creates the scenario
* The great things about creating RP scenarios is that newbies get to work directly with seniors, they join the conversation as a peer, and everyone can hear each other reason/think out loud
* Disaster RP and the group conversation it encourages lets everyone learn about how other people walk through a problem
* Break real things.  Fix real things.
* **Take a cluster/server out of rotation, create a problem, feed it fake traffic, and have the newbie try to fix it**
* The key is they are using **real production tools, systems, and methods** - not synthetic ones
* If you can't use real production systems use dev/test/stage or set up an environment.  The cost of the new environment will greatly outweigh the cost of an untrained newbie
* Ride shotgun as much as possible
* Clone incoming alerts during business hours so newbies can work with seniors as they fix real problems
* If possible, let the newbie take the first crack at the problem
* Or, if that's not possible, have both the junior and senior work in parallel
* After the outage is resolved, have the senior ask the junior questions about what happened
* The best education is continuous education.  Make learning systemic
* During a new product launch both juniors and seniors are **equally inexperienced** with the new product.  Have them work together during the launch
* Don't just write documentation, training, and post-mortems - record videos of training sessions
* "You have to **scale your humans** faster than you scale your machines"
* **Teach commonalities first** so newbies can build a foundation
* A good starting point is to **map out how a query or request flows through your systems**
* Give newbies a tour of the monitoring
* How do disaster conditions effect your stack?
* Seniors should teach juniors about tools that **help the juniors map systems/pipelines**
* If there is no formal training/process have a junior map out the system to the best of their knowedge and then a senior can work with them to fill in the gaps
* Have juniors rewrite/update documentation.  After they are done they can work with a senior to fill in the gaps or rewrite incorrect documentation