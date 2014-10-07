---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 4"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Part 4 of my notes from the Ruby course at Code Academy.

* Remember, **methods** are blocks of code to perform a specific task (reverse, upcase, downcase, custom stuff, etc)
* Why use methods?
	* Makes programs easier to debug (you can isolate problems faster)
	* Makes code more re-usable (why recreate the wheel and manually create an upcase code block for each new script?)
* Methods are defined using the ``def`` keyword
* Methods have 3 parts:
	* The **header** (including the word ``def``) and any **arguments** that the method takes
	* The **body** which is the actual code of the method
* Methods end with the ``end`` keyword, just like a ``do``
* If you try to call a method that doesn't exist (or you make a typo) you will get a **NameError**
* An example of a method:

		#!/usr/bin/ruby
		def square_it(x)
		  puts x ** 2
		end
		square_it 20
		
* The output from the above method is 400 (20 to the second power, aka 20 times 20)
* When creating a method you can use **splat arguments** to tell Ruby that there may be more than one argument ``def foo_method(*inputs)``
* Within a method you can use ``return`` to return a value to the caller.

		#!/usr/bin/ruby
		def triple(x)
		  return x * 3
		end
		
		foovar = triple(3)
		puts foovar

* The above example gives an output of 9
* An example of passing 2 arguments to a method:

		#!/usr/bin/ruby
		def add(x, y)
		  return x + y
		end

		foovar = add(3,2)
		puts foovar
		
* The output will 5 (aka 3 + 2)
* It's a Ruby best practice to end method names that produce boolean values with a question mark.
* **Blocks** are a way of creating methods that don't have a name (Similar to anonymous functions in JavaScript or lambdas in Python)

That's enough for today, watched MNF and I'm getting sleepy.