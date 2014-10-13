---
layout: post
title: "Notes from Learn Puppet Part 1"
description: ""
category: 
tags: ["puppet", "configuration management"]
---
{% include JB/setup %}

Puppet provides a "quest guide" (PDF) and a self contained VM for learning Puppet on [this page](https://docs.puppetlabs.com/learning/).  I'm going to spin up the VM, go through the quest guide, and take notes as I go.

* Puppet describes machine configurations in a declarative language known as **Puppet DSL**
* Like Chef, Puppet brings your systems to the desired state and keeps them there
* Puppet Enterprise (PE) includes a web console (reports, controlling the infra), orchestration (MCollective I assume), cloud provisioning tools (Razor I would guess), and professional support
* ``puppet -V`` displays the version number
* Other PE-only features: RBAC, event inspection, certification managment, and VMware cloud provisioning
* **Puppet Forge** is where the community shares modules and other Puppet related code
* A **module** is a self-contained bundle of code and data
* Puppet Enterprise also supports "PE supported modules" that are tested, supported, and maintained by Puppet Labs
* ``puppet install module module_name``
* The default PE module path is ``/etc/puppetlabs/puppet/modules``
* The ``modulepath`` is set in ``/etc/puppetlabs/puppet/puppet.conf``
* A **class** is a named block of Puppet code
* The process of applying a *class* to a *node* (or nodes) is called **classification**
* The power of classes is you can take them and apply them to any number of node(s), thus re-using the code
* **Facter** gathers facts about the system.  Facts are available in **manifests** via variables.  Facter is cross-platform.
* Use the ``factor`` command (or ``facter -p`` to dump facts.  Use ``facter item_name`` to get the value of a single item (ipaddress, swapfree, uuid, etc)
* The Puppet **agent** is the client process (``ps -ef | grep agent``)
* Every 30 minutes (by default) the *agent* contacts the master, requests a **catalog** and applies it.
* The process of pulling down and applying a catalog is called a **run**.
* ``puppet agent --test`` tells the Puppet client to download and run the catalogs.  Without the ``--test`` it goes into daemon mode.
* Puppet's DSL (domain specific language) can be considered **self-documenting code**
* **Resources** are the units that Puppets take action on, such as installing a package or starting a service
* The block of code that describes a resource is called a **resource declaration**
* Resource declarations are written in Puppet's DSL
* In a block of Puppet code it's considered a best practice to have a trailing comma after the final attribute/value pair
* **Resource types** (or just **type**) is a type of resource such as a file, user, package, service, group, etc
* ``puppet describe --list`` shows all available resource types on a system
* ``puppet describe resource_type`` shows all options for a given resource type
* ``puppet resource resource_type resource_name`` shows all settings for *resource_name* of type *resource_type*.  eg: ``puppet resource user root``
* A resource's **title** (such as root) must be unique (eg: can't have 2 services with the same name, duplicate user names, duplicate packages, etc)
* Each resource has a list of **attribute value pairs**, similar to key/value pairs.  These attributes are specific to the resource type (eg: the package resource type won't have a home directory attribute but a user will)
* **Providers** are what abstracts resources.  For example, there's no need to know the package management commands for each OS type, Puppet calls the provider and the provider does the work
* The **Resource Abstraction Layer (RAL)** is the term for resource types and the providers that implement them
* Resource declarations are stored in **manifests** (eg: start apache)
* Manifests have a ``.pp`` file extension, they are plain text
* You can use the command ``puppet parser validate mainfest_file.pp`` to check the syntax of a manifest
* ``puppet apply manifest_name.pp`` applies a local manifest
* You can use the ``--noop`` option to have Puppet show you the output but not make any changes, used to simulate changes
* The default manifest is ``init.pp``
* Variables must be added to a manifest before you can use it
* **Variable interpolation** allows you to replace occurrences of the variable with the value of the variable (``word = test ; echo this is a ${word}``)
* The ``file`` type also includes directories
* The variables in Facter can be considered built-in variables
* Facter variables use the syntax ``${::ipaddress}``.  The ``::`` deonotes a **top-scope variable**
* Puppets supports conditionals such as ``if unless case selector``
* With ``if`` statements, just like in Ruby, it checks the ``if``, then any ``elsif`` (if an elsif exists), then does the ``else`` (if the else exists)
* Manifests can support a ``warn()`` statement to log a message to the server at ``warn`` level
* Manifests also support the ``fail()`` statement
* Just like in Ruby, an ``unless`` block of code only executes if the condition is false
* With ``case`` statements, unlike Ruby, you use ``default:`` as the "use when nothing matches" block
* A neat trick is to use ``case`` to set variables, pseudo-code ``if OS = CentOS then $apache_pkg = httpd``
* **Selector** statements are similar to ``case`` but instead of executing code it assigns a value directly.  Here's an example:

		$rootgroup = $::osfamily ? {
		  'Solaris' => 'wheel',
		  'Darwin' => 'wheel',
		  'default' => 'root',
		}

* In the example above if we run it on a Solaris system it will set ``$rootgroup = wheel``
* Because selectors don't execute code directly you can't use ``warn()`` or ``fail()``
* You can also use selectors within resource blocks to do something like this:

		file { '/root/architecture.txt' :
		  ensure => file,
		  content => $::architecture ? {
			'i386' => "This machine has a 32-bit architecture.\n",
			'x86_64' => "This machine has a 64-bit architecture.\n",
			}
		}

* Done for now.

That's enough for the moment.  Going to take a break for a few and then finish up!