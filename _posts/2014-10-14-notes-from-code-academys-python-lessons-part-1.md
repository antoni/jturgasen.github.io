---
layout: post
title: "Notes from Code Academy's Python Lessons Part 1"
description: ""
category: 
tags: ["python"]
---
{% include JB/setup %}

Time to start Code Academy's Python lessons.  For my needs, I think Python will be a much better tool than Ruby.  Ruby's alright but the perl'esque syntax of "5 ways to do things" isn't fun and I would probably only use it for Chef / Puppet.  Python's consistent patterns and it's practical implications for writing sysadmin tools puts it above Ruby for my purposes.

My Python notes will probably not be as verbose as my Ruby notes because the Ruby course was a superb programming refresher (and boy did I need it).  Let's go!

* Store booleans like this ``var = True`` or ``var = False``.  Notice the case.
* In Python **whitespace matters**, code won't run if you don't indent properly
* Instead of a bunch of ``#`` for comments you can start and end comment blocks with triple quotes ``"""``
* The ``**`` math operator is "power of"
* Supports mathematical order of operations
* Strings must be quoted.  If you quote a number it's a string, if you don't quote it it's a number (int, float, double, whatever)
* Just like shell/regex a backslash escapes
* Each character in a string is assigned a number starting at 0.  The number is called the **index** (think: arrays)
* This grabs the 3rd character out of the string ``foo = "CATS"[2]``
* Like Ruby, you can perform **methods** on strings (and I assume objects in general)
* String methods use the syntax ``method_name(var_name)``
* String methods can also use a Ruby'esque syntax of ``var_name.method_name()``.  **THIS ONLY WORKS WITH STRINGS**
* The ``str()`` method turns non-strings into strings eg ``str(1234)``
* Weird print syntax ``foo = bar ; print foo`` (no quotes or dollar sign or anything)
* You can concatenate strings with a ``+`` like this ``"foo" + "bar"``
* To print a variable within a string you can do something like this:

		#!/usr/bin/python
		string_1 = "Camelot"
		string_2 = "place"
		print "Let's not go to %s. 'Tis a silly %s." % (string_1, string_2)

* ``from datetime import datetime`` seems to say "import the datetime method from the library named datetime"
* In the example below notice how we are calling the methods hour, minute, and second:

		#!/usr/bin/python
		from datetime import datetime
		now = datetime.now()
		print '%s:%s:%s' % (now.hour, now.minute, now.second)
		
* Use ``==`` to compare because ``=`` is used to assign values
* When doing number comparisons you can do something like ``foovar = 99 = (98 + 1)``.  Notice the parenthesis.   The end result sets ``foovar`` to ``True``
* Reminder: ``and`` returns true if the expressions on both sides are true and ``or`` returns true if the expression on one side is true
* The ``not`` operator returns True for false statements and False for true statements
* Boolean operators support **order of operations** just like mathematical operations.  They are evaluated in this order ``not and or``
* When using boolean operators you can use parenthesis to ensure expressions are evaluated in the order you want

That's enough for tonight, conditionals tomorrow.