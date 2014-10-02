---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 2"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Part 2!

* Methods that end with a ``?`` evaluate to true or false
* ``gsub`` method is like **sed**
* Using a ``!`` (eg: ``foovar.upcase!``) modifies the variable in-place and is called an **unsafe method** or **dangerous method**
* Unsafe methods "change state that someone else might have a reference to"  https://stackoverflow.com/a/612196
* ``while`` loops look similar to shell:

		#!/usr/bin/ruby
		counter = 1
		while counter > 11
			puts counter
		counter = counter + 1
		end

* ``until`` loops look similar to shell too:

		#!/usr/bin/ruby
		i = 0
		until i == 10
			i += 1
		end
		puts i

* Notice in the above example how they increment the variable using ``+=`` instead of ``i = i + 1``
* **Assignment operators** change the variable in-place:
	* ``i += 5`` increments i by 5
	* ``i -= 3`` decrements i by 3
	* ``i *= 2`` multiplies i by 2
	* ``i /= 4`` divides i by 4
* ``for`` loops are somewhat similar to shell:

		#!/usr/bin/ruby
		for i in 1...100
		puts i
		end

* Note that the above script prints ``1...100`` as the final line.  This is called **implicit return** and can be suppressed
* The syntax ``1...10`` excludes 10 but the syntax ``1..10`` includes 10
* An **iterator** is a method that repeatedly invokes a block of code (it "reiterates" it)
* You can create a simple loop with the ``loop`` method
* Similar to shell you can use ``break`` to break out of a loop with something like ``break if foovar > 5``
* Instead of ``do`` and ``end`` you can use curly braces.  ``{`` means do and ``}`` means end

Alright I'm tired, that's enough for tonight.