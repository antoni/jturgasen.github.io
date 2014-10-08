---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 5"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Part 5 of my notes from Code Academy's Ruby course.

* ``array_name[0..-1]`` is used to process all elements of an array
* A method can take a block as a parameter
* "Passing a block to a method is a great way of abstracting certain tasks from the method and defining those tasks when we call the method"
* Something like ``sort`` will sort an array, but ``sort!`` will *destroy* the existing array and replace it with the sorted data
* ``<=>`` is the **combined comparison operator**
* ``<=>`` returns 0 if the first operand equals the second, 1 if first operand is greater than the second, and -1 if the first operand is less than the second
* You can set default parameters for an array like this: ``def array_name(arg1, arg2=foo)`` .  The default for the 2nd arguments is foo.
* In key-value pairs don't forget that keys must be unique but values can be repeated
* Two ways to create a hash.  The first is **hash literal notation** like ``hash_name = { "key" => "value" }``
* The other is **hash constructor notation** like this ``hash_name = Hash.new``
* An example of iterating over a hash:

		#!/usr/bin/ruby
		my_hash = { "food" => "grapes"
		  "drink" = "tea"
		  "treat" = "candy"
		}
		my_hash.each { |category, thing| puts thing }

* The example above will print grapes tea candy.  If we changed ``puts thing`` to ``puts category`` it would print food drink treat.
* **Nil** means *nothing at all* while **false** means *not true*
* If you try to match a key that does not exist in the hash it will return nil
* Or, you can create a default key like this ``hash_name = Hash.new("foo")`` to return foo if an invalid key is searched on

Got a late start tonight, more tomorrow.