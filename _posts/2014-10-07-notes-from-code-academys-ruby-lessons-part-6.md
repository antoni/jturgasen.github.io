---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 6"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Guess I can't learn an entire language in 5 days.  Who knew!?

* There can be multiple different strings that all have the same value, there's only one copy of any particular **symbol** at a given time
* Symbols start with a colon, eg: ``:foo_symbol``
* The ``.object_id`` method gets the ID of an object (think UUID).  **Object IDs** are how Ruby knows if two objects are the same (or unique)
* Putting these two ideas together, two of the same strings will have the same object ID but two of the same symbols will have unique IDs
* In general, symbols are primarily used either as hash keys or for referencing method names
* Symbols are immutable
* Symbols-as-keys are faster than strings-as-keys
* To switch symbols to strings and vice versa use the ``.to_sym`` and ``.to_s`` methods
* You can also use ``.intern`` instead of ``.to_sym``
* The ``.push`` method appends objects to the end of an array
* When you create a hash and use the symbol ``=>`` the symbol is called a **hash rocket**
* In Ruby 1.9 the hash syntax has changed!  Instead of a hash rocket it looks like this:

		#!/usr/bin/ruby
		my_hash = { item1: "value"
		  item2: "foo"
		  item3: "blah"
		}

* In the example above note how the hash rocket is missing and the colon to denote a symbol comes *after* the key
* The ``benchmark`` module can measure code execution time!
* You can use the ``.select`` method to filter a hash for values that meet a specified criteria (starts with "a", >50, etc)
* ``.each_key`` and ``.each_value`` methods can be used to iterate over keys or values only
* Ruby supports good old fashioned **case statements** like this:

		#!/usr/bin/ruby
		puts "type a letter:"
		input = gets.chomp.downcase
		case input
		when 'a'
		  puts "you typed a"
		when 'b'
		  puts "you typed b"
		else
		  puts "you did not type a or b"
		end

* To add an item to a hash use something like this:

		my_hash = {:a => 5}
		my_hash[:key] = "value"

* The ``.to_i`` method changes a string to an integer
* To delete a key/value from a hash use something like ``my_hash.delete(key_name)``
* **CRUD** stands for create, read, update, delete.  They are the basic functions of things like DB actions, interacting with a web site, etc
* Instead of using a long-form ``if`` or ``unless`` block you can do something like this:

		#!/usr/bin/ruby
		puts "foobar" if true
		puts "foo2" unless false

* A **ternary conditional expression** is an if / else statement.  It's made up of a boolean (true/false), an expression to evaluate if the boolean is true, and an expression to evaluate if the booloean is false
* An example of a ternary conditional expression is ``puts 3 < 4 ? "3 is less than 4!" : "3 is not less than 4."`` (boolean, true, false)
* You can write compressed ``case`` statements like this:

		#!/usr/bin/ruby
		case variable
		when "foo" then puts "bar"
		when "bar" then puts "foo"
		else puts "neither!"

* placeholder
		
That's enough for tonight!