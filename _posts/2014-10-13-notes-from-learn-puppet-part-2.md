---
layout: post
title: "Notes from Learn Puppet Part 2"
description: ""
category: 
tags: ["puppet", "configuration management"]
---
{% include JB/setup %}

Part 2, here we go!

* In Puppet resource declarations are NOT executed in order
* A **catalog** is a compilation of all resources that will be applied to a system
* Unless you explicitly specify the relationship between resources Puppet will manage them in it's own order
* Resource relationships that are implied are **implicit** (eg: create user before creating a subdir in it's homedir)
* Possible **metaparameter attributes** are ``before require notify subscribe``.  They use the ``attribute => value`` syntax.
	* ``before`` - Resource A is applied before Resource B
	* ``require`` - Resource A is applied after Resource B
	* ``notify`` - Same as ``before`` but will generate a refresh when the resource changes (conf file resource tells web server resource when conf file changes)
	* ``subscribe`` - Same as ``require`` but the subscribing resource will refresh if the target resource changes (web server watches conf file, bounces when changed)
* When using attribute => value metaprameters the value is the title(s) of one or more target resources
* ```package/file/service`` is an important pattern.  Package installs, file depends on package (if no Apache package, don't put httpd.conf in place), and service watches for (subscribes) to changes in the conf file
* **Classes** are named blocks of resuable Puppet.
* In practice you might create a class for each service, or for a special user that should exist on certain/all system(s)
* A **class definition** is the actual code while **declaring a class** is the process of actually using it in a manifest
* Another way to define a class is something like this:

		class { 'apache':
		  default_vhost => false
		}

* The example above sets one parameters within the class.  When using an ``include`` statement you pass nothing, thus you only get defaults
* Best practice: Always separate a class definition from the declaration.  This way the definitions are re-usable
* Variable declarations contained in a class definition can be called **class parameters**
* Variable declarations within a class represent the **default values** and are used if nothing is passed in (eg: no arguments/options)
* "Resources and classes are the atoms and molecules, and modules are amoeba, a self contained organism"
* In practice every manifest should be contained within a module
* A **module** is a directory with a specific structure
* ``puppet agent --configprint config_option`` shows the setting for ``config_option`` (eg: modulepath)
* The top directory of a module should always be the module's name
* Module **tests** allow you to apply and test classes defined in your module without having to deal with higher level configuration tasks like node classification
* In the ``modules/module_name/manifests`` directory you must have the ``init.pp`` manifest.  It contains the module's main class
* If you declare a class multiple times Puppet will declare the first one and skip subsequent declarations.  This way you can declare it in multiple places and not have to worry.
* **Node classifer** states which classes apply to which nodes
* To classify a node...
	1. There must be a module with the same name as the class
	2. The module must have the ``init.pp`` in place
	3. The ``init.pp`` must contain the definition of the class
* ``puppet module`` can be used to interact with modules, pull modules from the Forge, create module templates, etc
* Modules on the Forge are in the format ``authorname-modulename``

That's it!  I have a lot more learning to do but it's a good start.