---
layout: post
title: "Notes from Code Academy's Ruby Lessons Part 1"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

Time to get formal with my Ruby education.

Here are my notes from the Ruby course at Code Academy.  Remember, I'm mostly a shell (bash/ksh/sh) guy so don't laugh too much!

* In Ruby, everything is an **object**.
* Think about **data types** (numbers, booleans, strings)
	* Numbers are numbers but they haven't talked about precision or specific types yet (float, double, integer, etc)
	* Booleans are true or false (eg: 0 or 1)
	* Strings are text strings (eg: "blah blah blah" or '1 + 2 * 3')
* Never use quotes with booleans or else Ruby thinks it's a string
* Ruby is **case-sensitive**
* When setting variables note the **spaces** around the equals sign, for example: ``my_num = 25``
* Supports all of the expected math operations:
	* \+ is addition
	* \- is subtraction
	* \* is multiplication
	* / is division
	* ** is exponent
	* % is modulo (the remainder of division)
* ``print`` does not add a newline after a string but ``puts`` does
* Everything in Ruby has **methods**.  For example, strings have built in methods to tell string length, reverse the string, etc
* Methods are called by using a period.  For example, ``"test test test".length`` returns 14.  You are performing the method named *length* on the string "test test test"
* They're showing me more string methods, I won't put more examples here, Google is my friend
* Comments start with a ``#`` or you can do **comment blocks** by starting them with ``=begin`` and closing them with ``=end``
* **Local variables** start with a lowercase letter or underscore and words should be separated by underscores.  This is not enforced but it's the convention.
* You can **chain together** methods like this: ``"Josh T".upcase.reverse``
* ``gets`` = read string.  Note that Ruby adds a newline when reading a string!
* ``gets.chomp`` = remove the newline at the end of the string
* Variables use a ``#`` instead of a ``$`` and are surrounded by curly brackets.  For example: ``#{first_name}``
* To apply a method to a variable use something like this: ``foovar.upcase!`` .  This applies the upcase method to the variable foovar.  Note the bang at the end.
* **Whitespace** is ignored but the convention is to indent by two spaces
* ``if`` statements look like you would expect them to but are closed with ``end`` instead of ``fi`` and don't use a ``; then``

		if 1 > 2
		  print "You won't see this"
		else
		  print "One is not greater than two"
		end

* ``elsif`` looks like you would expect it to, too:

		if x > y
		  print "x is greater than y"
		elsif y > x
		  print "y is greater than x"
		else
		  print "x is equal to y"
		end

* ``unless`` tests for **falseness**.  You can use this instead of switching your ``if / else`` statements.  Close it with an ``end``
* You can compare things with ``==`` and ``!=`` (eg: if var1 == var2 then do this or if var1 != var2 then do this)
* Also supports syntax similar to shell with ``> => < <=``
* ``&&``, ``||``, and ``!`` are called **boolean operators**
* ``&&`` is an **and** test.  It returns true if the statements on both sides of it are true.
* ``||`` is an **or** test.  If one or both sides of the statement are true it returns true.  If one or both sides are false then it returns false.
* ``!`` is a **not** operator, for example the result of ``!true`` is false.
* You can chain and combine boolean operators by using parenthesis

That's enough for today, more soon.