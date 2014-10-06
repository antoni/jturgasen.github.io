---
layout: post
title: "Notes from Chef Fundamentals Part 4"
description: ""
category: 
tags: ["chef"]
---
{% include JB/setup %}

Notes from the [Chef Fundamentals](http://learn.getchef.com/fundamentals-series) series of webcasts, videos #4 and #5.

* **Data Bags** are generic, arbitrary stores of information about the infrastructure
* Data Bag items are in JSON format
* You can use Ruby to iterate through a data bag, pull info from it, and then apply that info to a file/recipe/erb/etc
* ``knife upload data_bag_directory``
* **Resource Guards** are
* Use 2 spaces instead of 4 spaces or a tab for Chef and Ruby
* Another way to embed Ruby is to open with ``<%`` and end with ``%>``.  This allows you to do tests/conditionals.
* ``knife diff path_to_cookbook``
* You can increment cookbook versions
* An **environment** is the way you model the lifecycle of your environments
* For example, you could have a dev environment, a QA/test/staging environment, and a prod environment
* You can have multiple environments within one organization
* ``knife environment show env_name``
* There is a ``_default`` environment
* Environments can also provide attributes and default attributes
* For example, dev might point to one yum repo while prod points to another
* You can specify the version of a cookbook that is used in an environment
* ``knife cookbook show cookbook_name``
* By default Chef pulls down the latest version of a cookbook
* ``knife environment list``
* ``knife environment from file env_file_here.rb``
* ``knife search node "platform:centos"``
* You can use ``knife ec2`` to spin up a new server and push it's Chef config