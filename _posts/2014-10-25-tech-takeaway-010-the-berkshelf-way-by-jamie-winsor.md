---
layout: post
title: "Tech Takeaway 010: The Berkshelf Way by Jamie Winsor"
description: ""
category: 
tags: ["tech takeaway", "chef"]
---
{% include JB/setup %}

[Jamie Winsor talks about stuff I don't know yet I haven't started watching it it's from ChefCon or something](https://www.youtube.com/watch?v=hYt0E84kYUI&list=UUxEieNpB_tXiUBoF9zkPmAw&index=72)

* Berkshelf manages cookbook and it's dependencies outside of the Chef repository
* Berkshelf is CLI, source code management + package management + it replaces portions of Knife
* Recipes contain resource collections.  Recipes are put into cookbooks.  Cookbooks are "put" into Chef Solo or Chef Client
* Berkshelf is a Ruby Gem
* Can import existing cookbooks
* Be sure to set the cookbook name in the ``metadata.rb`` file or else it will just use the directory name
* Cookbook versions are in the format v0.0.0.  Major, minor (features), and bug fixes
* You can also set your supported platforms in the metadata (RHEL, CentOS, Ubuntu, etc)
* Set dependencies in your cookbook
* Berkshelf keeps your cookbooks in your homedir, weird
* ``berks upload`` pushes the cookbooks to your Chef server
* "Readme driven development".  Before you start coding create a readme, show it to other developers, and ask "does this make sense?"
* Private Recipes are not exposed to the end user.  Never put in the run_list.  Always included in other recipes.  Documented in the code for other devs
* Public Recipes are put in the run_list, documented in a README, documented in the metadata
* Data-Driven Cookbooks allow you to change the cookbook's behavior at run time (or tune the application)
* 3 ways to configure an application: attributes, databags, encrypted databags
* He feels that attributes provide the path of least resistance for cookbook consumers, they provide sensible defaults, and you can configure your app on an environment (dev/qa/prod) level
* He thinks databags should be used at an *organizational* level configurations (users/groups/yum/apt).  Anything that's configured by your "base" cookbook
* He doesn't like roles because...
	* Not versioned
	* Can't be packaged / distributed
	* Not namespaced
	* They are organizational level data (encompasses dev/qa/prod)
* Validate your data bags!
* Library cookbooks are a design pattern that...
	* Might contain recipes, might not
	* Contains LWRPs, definitions, and/or libraries that are useful to other cookbooks
	* The yum cookbook is a perfect example.  Contains a couple LWRPs and some common recipes
* Wrapper cookbooks are a design pattern that...
	* Contain 1 or 2 or 3 recipies, lightweight
	* Contains attribute overrides
	* Acts as an abstraction!
* Super quick workflow that they use:
	* Edit cookbook
	* Spin up VM with Vagrant
	* Test / validate
	* Repeat
* Vagrant creates VMs on your local computer, so super fast and easier than using the hypervisor's native tools
* I should probably read about Vagrant