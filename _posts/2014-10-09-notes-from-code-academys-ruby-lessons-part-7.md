---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 7"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Part 7, here we go!

* ``||=`` is the **conditional assignment operator**
* The conditional assignment operator is used to assign a value to a variable *only* when the variable has not been set.  In other words, it makes the value of the variable "static"
* If a method doesn't have an implicit ``return`` it returns the result of the last evaluated expression
* **Short-circuit evaluation** is when Ruby looks at only *one* side of a ``||`` or ``&&`` because it already knows the end result.  Reading the other side is unnecessary.
* You can use the ``.times`` method to repeat something X number of times, eg: ``10.times { puts "foo" }``
* If you want to repeat an action on each element of an array you can use something like ``[1, 2, 3].each { |y| puts y * 5 }``
* You can use the ``.downto`` and ``.upto`` methods to specify a range ``90.upto(100) { |x| puts x }``
* **Call and response** is how Ruby figures out if a method is a valid way to manipulate and object.  This returns false: ``[1, 2, 3].respond_to?(:to_sym)`` because you can't turn an array into a symbol
* Instead of using the ``.push`` method to add objects to an array you can do something like this instead: ``[1, 2, 3] << 4``
* **String interpolation** is when you add the value of a variable to the end of a string like this:

		#!/usr/bin/ruby
		food = "grapes"
		puts "I love " + food
		# or
		puts "I love " << food
		# or you can do multiples like
		puts "I love " + food + " with peanut butter"
		# but, the real way you should do it is...
		puts "I love #{food}."

* When using non-strings for string interpolation you need to use the ``.to_s`` method to switch them to strings (eg: ints to strings)
* Instead of doing a typical ``if`` block you can do ``puts "foo" if 2 > 1``
* Instead of doing a typical ``if else`` block you can do something like:

		#!/usr/bin/ruby
		foo = 4
		puts foo == 4 ? "Yes" : "No"
		# If foo = 4 then print Yes, if not, print No

* Instead of a bunch of ``if elsif else`` lines use ``case`` instead
* To add a module to a script put ``module 'foo'`` at the beginning

More tomorrow!