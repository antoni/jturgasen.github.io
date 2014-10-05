---
layout: post
title: "Notes from Chef Fundamentals Part 3"
description: ""
category: 
tags: ["chef"]
---
{% include JB/setup %}

Notes from the [Chef Fundamentals](http://learn.getchef.com/fundamentals-series) series of webcasts, video #3.

* ``knife bootstrap hostname_here -x root -P root_password_here -N node_name_here -r "recipe[apache]"
* **Node objects** are representations of a node in JSON format
* Nodes can have properties such as...
	* Attributes
	* Run List
	* Chef Environment
* Node object data is stored in a DB and is added to the "search index thinger"
* Node object data is searchable via API (knife and recipes)
* You can add data to a node object using attributes in cookbooks, roles, directly on a node, etc
* **ohai** takes an inventory of the system and omits some JSON data
* Node objects are stored as **hashes** (key/value pairs), most are strings
* You can use attributes to do stuff like this in your index.html templates:

		<h1>Hello World!</h1>
		<p>My name is <%= node['hostname'] %>.</p>

* The example above uses **embedded Ruby**
* **.erb** files stand for embedded Ruby
* ``<%=`` starts an embedded Ruby statement and ``%>`` ends it
* You can use the ``ohai`` command on a node to dump a bunch of system details/attributes.  Presented in JSON.
* You can also use something like ``ohai hostname`` to only display the value of the hostname attribute, ``ohai swap``, ``ohai memory``, etc etc
* ``knife node show node_name_here`` shows some info remotely (vs ohai which is local)
* ``knife node show node_name_here -a attribute`` matches a specific attribute (hostname, swap, OS version, etc)
* Attributes can be set at various levels.  Here they are in decreasing (first is highest) order of prescedence:
	* On the node itself via ohai
	* Roles
	* Environment
	* Cookbook recipes
	* Cookbook attribute files
* Attributes in cookbook files can be set at ``./cookbooks/cookbook_name_here/attributes/default.rb``
* The format for attributes in cookbooks is prescedence, attribute name, attribute value, eg:

		default['apache']['dir'/] = "/etc/apache2"

* In the example above ``default`` is the precedence, ``['apache']['dir']`` is the attribute name, and ``"/etc/apache2"`` is the attribute value
* Also, ``['dir']`` is known as a **sub-key** or **sub-hash**
* **Roles** are the role of a server/node (web server, DB server, monitoring server, etc)
* Roles are contained in files within the ``roles`` subdirectory of the ``chef-repo`` directory
* Role files are JSON
* Roles can contain a run list
* Example role file:

		{
		  "name" : "webserver",
		  "default_attributes" : {
		    "apache" : {
			  "greeting" : "Webinar"
			}
		  },
		  "run_list" : [
		    "recipe[apache]"
		  ]
		}
		
* Roles...
	* Must have a name
	* Must have a description
	* May have a run list
	* May set node attributes (default_attributes, override_attributes)
* ``knife role from file webserver.json`` uploads it to the server
* When using roles make sure you don't duplicate things (eg: node has apache in run list, you change the role to websrv that contains apache, now it's duplicated)
* ``knife node run list remove host_name_here "recipe[apache]"`` removes an entry from the run list
* ``knife node run list add host_name_here "role[webserver]"``
* Attributes can be set in multiple places for flexibility (within roles, in cookbooks, in recipes, ohai, environment attributes, etc)
* Best practice: Set a sane default attribute that will be used in a cookbook
* Attributes in roles override attributes in cookbooks
* 