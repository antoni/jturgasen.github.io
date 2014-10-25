---
layout: post
title: "Tech Takeaway 011: Lessons from Etsy: Avoiding Kitchen Nightmares"
description: ""
category: 
tags: ["tech takeaways", "chef"]
---
{% include JB/setup %}

[From ChefConf 2012](https://www.youtube.com/watch?v=nSnJCJiZDDU&list=UUxEieNpB_tXiUBoF9zkPmAw)

* Documented standards and communicated best practices (wiki page, hands-on training before using it)
* Robust testing workflow
* Linting with rules derived from your standards (Foodcritic)
* Chef testing workflow...
	* Standard prod dev test
	* Take a real production node and remove it from the production "pool" and flip it into the test pool
	* Do your testing, then flip it back to production
	* Neat!
* ``knife-env-diff`` to compare environments, helps keep dev and prod in sync
* ``knife-spork`` has 4 stages...
	* Check (looking at versioning info for a cookbook)
	* Bump (auto increment the cookbook's version number)
	* Upload (Knife upload and freeze)
	* Promote (Set environment constraints equal to the specified version)
* You can **freeze** a cookbook to prevent updates
* Beware automatic package upgrades!
* Be specific with your recipes so you don't get bit by upgrades if you know you need a specific version
* Instead of using Chef to upgrade packages do an install and specify the version.  Upgrade leads to way too many unknowns.
* When collecting metrics find a way to also include Chef pushes so you can see if Chef is the culprit
* Monitor Chef run times
* Create an "out of band" server in case the Chef server is down.  Because it is down you can't use Knife.  They use ``dsh``
* Be careful with configs that are distributed with packages.  They may overwrite Chef-generated configs.  Make sure Chef replaces them before you bounce the service, watch the resource order
* Can you replicate your on-site Chef to Hosted Chef as a DR measure?