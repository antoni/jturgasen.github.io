---
layout: post
title: "Notes from Learn Chef Part 1"
description: ""
category: 
tags: ["chef"]
---
{% include JB/setup %}

Notes from Opscode's [Learn Chef](https://learn.getchef.com) tutorial.  Looks like there won't be a part 2, I finished it!

* **chef-apply** allows a single recipe to be run from the command line
* Recipes are Ruby ( .rb ) files
* A recipe declares what state each resource should be in but not how to achieve that state - it's abstracted
* A recipe stops if any step fails
* For example, you can use a single recipe to install, configure, and start a web server
* Chef looks at the current configuration state and applies the action only if the current state doesn't match the desired state.  Example:
	1. Create a simple MOTD recipe (test.rb)
	2. Run ``chef-apply test.rb``
	3. The MOTD file is generated
	4. Run ``chef-apply test.rb`` again
	5. The MOTD file is NOT regenerated because it matches the desired state (exists and has the desired text)
* If a file is changed Chef will change it back to it's original state.  For example:
	1. Create a MOTD recipe in the file ``test.rb``
	2. Run ``chef-apply test.rb``
	3. The MOTD file is generated
	4. Edit the MOTD file and change it's text
	5. Re-run ``chef-apply test.rb``
	6. The contents of the MOTD file will be replaced with the correct text
* A **file** is a type of resource
* Resources have actions (eg: file delete, service start, package install)
* Resources also have a **default action**, and it's usually an affirmitve one (eg: NOT file delete)
* Resources are applied in the order they are specified in the recipe
* The following example installs the httpd package, enables it, and starts it:

		package 'httpd'
		
		service 'httpd' do
		action [:start, :enable]
		end

* Note how in the example above we specify multiple actions on the same line (start + enable)
* So far we have worked with the file, package, and service resources.  The **template** resource is coming up soon.
* A **cookbook** provides structure to your recipes and enables you to more easily reference external files
* ``chef generate cookbook learn_chef_httpd`` creates a directory called learn_chef_httpd and lays out default versions of the cookbook's files
* ``chef generate template learn_chef_httpd index.html`` creates a **template** for your index.html
* Note that it actually creates a file named index.html.erb
* Files with a .erb extension mean that the file can have **placeholders**
* **chef-client** is the agent/daemon that runs on a client node, performs the steps that are required to bring the node to the expected state
* Note that **chef-apply** is used to run a single recipe and **chef-client** is used to run an entire cookbook
* The **run-list** lets you specify which recipes to run, and the order in which to run them
* A **Chef server** acts as a central hub for your cookbooks
* A **node** is any computer that is managed by a Chef server (physical or VM)
* The **Chef Supermarket** contains pre-configured cookbooks
* **knife** is a command-line tool that provides an interface between a local chef-repo and the Chef server
* The actual term for a Chef client is a **node**
* **Bootstrapping** is the process of installing the chef tools on a (client) node and checking in / registering with the Chef server
* The Chef server also has a **management console** / GUI
* **Dynamic configuration** means that you can write a single, general recipe that’s customized for a particular node as the recipe runs.
* Dynamic configuration is enabled by using templates
* **Node object** contains a number of attributes that describe the node, and these attributes are saved on the Chef server