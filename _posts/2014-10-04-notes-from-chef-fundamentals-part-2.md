---
layout: post
title: "Notes from Chef Fundamentals Part 2"
description: ""
category: 
tags: ["chef"]
---
{% include JB/setup %}

Notes from the [Chef Fundamentals](http://learn.getchef.com/fundamentals-series) series of webcasts, video #2.

* The **knife bootstrap** command installs the Chef client software onto a node
* ``knife bootstrap hostname_here -x root -P root_password_here -N node_name``
* -x = username, -N = node name, used by Chef server instead of the FQDN
* ``knife.rb`` seems to be the knife configuration file.  Contains stuff like:
	* chef_server_url
	* validation_client_name
	* validation_client_key
* The 3 items above are passed to the client when using knife bootstrap
* After the client receives the ``knife bootstrap`` it...
	* Installs, configures, and runs chef-client
	* Registers the node with the Chef server and sends node details
	* The Chef server saves the node details in a DB and updates the search information
* ``/etc/chef`` holds the conf files including ``client.rb`` (client conf file) and the certificates
* Node details may also be called **node attributes** or **attributes**
* The good thing about running ``chef-client`` manually is you can see the output as it runs (I assume you can log this)
* A **cookbook** is like a "package" for Chef recipes.  Contains...
	* Recipes
	* Files
	* Templates
	* Libraries
	* etc
* Typically cookbooks map 1 to 1 to a piece of software or functionality
* ``knife cookbook create cookbook_name`` , run this command IN the repo directory!
* Stuff like package, action, file, mode, and so on can also be called **primitives**
* Resources are **declarative**, they say *what* they want to happen rather than *how*
* Resources take actions through *providers*.  yum, apt, pkg (Solaris), are providers for packages, and so on
* Chef looks at the **platform** (RHEL, Debian, Solaris, Windows, etc) to determine which provider to use
* Resources are executed *in order*
* When using a template you specify **attributes** or **parameters** (mode, template to use, owner, etc)
* ``knife cookbook upload cookbook_name`` pushes the cookbook from your workstation to the server
* ``knife node run_list add hostname_of_node_here "recipe[cookbook_name_here]"`` adds a cookbook to a node object on the server