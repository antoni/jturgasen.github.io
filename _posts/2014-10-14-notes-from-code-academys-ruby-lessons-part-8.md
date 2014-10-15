---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 8"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

I took a break for a few days but now it's full steam ahead.

* The ``.collect!`` method **mutates** the object with the results.  For example:

		#!/usr/bin/ruby
		my_array = [1, 2, 3]
		my_array.collect { |x| x ** 2}
		# The above example does NOT change the contents of the array
		my_array.collect! { |x| x ** 2}
		# The above example DOES change the array

* As a general rule, putting a ``!`` on the end of a method signifies it could do something dangerous or unexpected
* The ``yield`` keyword transfers control from the calling method to the block and back again
* Blocks are not objects.  This is one of the few exceptions to the "everything is an object" rule.
* Blocks can't be saved to variables
* A **proc** is a named block
* The advantage of procs vs blocks is code re-use.  You can pass a proc to a method instead of re-typing and re-passing the block to the method over and over
* To create a proc call ``Proc.new`` and pass in the block you want to save.  For example, this creates a proc called ``triple``

		#!/usr/bin/ruby
		triple = Proc.new { |x| x * 3 }

* Done for now.

Man I shouldn't have taken a break, I need to review my previous notes.