---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 3"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Day three, let's go!

* The ``next`` keyword "jumps to the next iteration of the most internal loop if the condition is met"
* Like shell, an **array** is a list of things (numbers, hostnames, etc).  ``my_array = [1, 2, 3, 4]`` or ``my_array = ["test test", "foo bar", "blah blah"]``
* You can use the ``.each`` iterator to apply an expression to each element of an object, one at a time:

		#!/usr/bin/ruby
		my_array = [1, 2, 3, 4]
	
		my_array.each do |x|
		  x += 100
		  puts "#{x}"
		end
		# prints 101 102 103 104

* Note how in the example above ``x`` serves as kind of a "temporary variable" to hold the current element of the array as the loop iterates through them
* When using strings in an array you need to quote them eg: ``my_array = ["a", "b", "c"]``
* The ``.times`` iterator performs an action a specified number of times.

		#!/usr/bin/ruby
		20.times { puts "foobar" }
		# prints foobar 20 times

* Note how we use curly braces instead of ``do`` and ``end``
* The ``.split`` method to take a string, split it at the specified place (such as a space) and then put the output into an array ``myvar.split(" ")`` splits space
* Each element in an array is called an **index**.  The first element is index 0.
* To grab the 4th element in ``my_array = [10, 20, 30, 40, 50]`` you would use ``my_array[4]`` and it would print 50.  Called **access by index**.
* This is called a **multidimensional array** ``[[1,2,3,4],[5,6,7,8],[9,10,11,12]]``
* **Hashes** are key-value pairs.

		#!/usr/bin/ruby
		my_hash = {
		  "key1" => "value1"
		  "age" => 20
		  "first_name" => "bob"
		}

* ``my_hash = Hash.new`` creates a new, blank hash
* Add new elements to a hash like this:

		#!/usr/bin/ruby
		josh_hash = Hash.new
		josh_hash["First Name"] = "Josh"
		josh_hash["Last Name"] = "Turgasen"
		josh_hash["Age"] = 99

* You can extract a value from a hash by specifying the key like this:

		#!/usr/bin/ruby
		josh_hash = Hash.new
		josh_hash["First Name"] = "Josh"
		puts josh_hash["First Name"]

* You can use the ``.each`` iterator over an array or hash
* For multidimensional arrays you can use something like this to grab a specific element:

		#!/usr/bin/ruby
		sandwiches = [["ham", "swiss"], ["turkey", "cheddar"], ["roast beef", "gruyere"]]
		puts s[0][1]

* The above example prints "swiss" (element 0 of the array, element 1 of the sub-array)
* When you create a hash you want to create a **default value** or **default_proc** that Ruby runs when you try to access a hash key that doesn't exist
* It seems like you always quote strings but never quote numbers / integers

That's enough for tonight, time to play with Chef.