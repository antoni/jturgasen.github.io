---
layout: post
title: "Simple Ruby Stuff"
description: ""
category: 
tags: ["ruby"]
---
{% include JB/setup %}

I'm playing around with Ruby for the first time tonight so I figured I would document what I'm doing to help me absorb it.  There's nothing formal here, I'm kinda jumping from source to source and picking up bits as I go and formalized learning will being later.

If you plan to work with Chef there's a great page called [Just Enough Ruby for Chef](https://docs.getchef.com/just_enough_ruby_for_chef.html) on Opscode's web site

--------------------------------

Here's a little program:

	#!/usr/bin/ruby
	puts "oops missing quotes"

Output:

	oops missing quotes

Now, let's test the syntax.  Think **c** for **check**:

	ruby -c ./test.rb

Output:

	Syntax OK

--------------------------------

Print a string:

	#!/usr/bin/ruby
	puts "test string"

Output:

	test string

Looks good.

--------------------------------

Trying to print a string without quotes:

	#!/usr/bin/ruby
	puts test test

Output:

	./test.rb:3:in `test': wrong number of arguments (0 for 2..3) (ArgumentError)

Whoops!  Looks like instead of printing ``test test`` it is trying to call the test function/command/routine/keyword whatever it's called.  Seems like we have to quote stuff like this.

--------------------------------

How can you tell which line has a syntax error?

	#!/usr/bin/ruby
	puts "one"
	puts "two"
	puts "three"
	puts "four
	puts "five"
	puts "six"
	puts "seven"

Let's run it:

	./test.rb:7: syntax error, unexpected tIDENTIFER, expecting end-of-input
	puts "five"
	          ^

Now let's modify it to break the "three" line instead of the "four" line:

	#!/usr/bin/ruby
	puts "one"
	puts "two"
	puts "three
	puts "four"
	puts "five"
	puts "six"
	puts "seven"

The output:

	./test.rb:6: syntax error, unexpected tIDENTIFER, expecting end-of-input
	puts "four"
			  ^

See this difference?  In the error the 7 became 6 and the "bad line" moved up.  We should start at the lines mentioned in the error (the line number and command) and work our way back.

--------------------------------

Let's play with strings and escapes:

	#!/usr/bin/ruby
	puts "It's a test"
	puts 'It's also a test'

Output:

	./test.rb:4: syntax error, unexpected tIDENTIFER, expecting end-of-input

We messed up our quotes.  Let's try to escape the apostrophe:

	#!/usr/bin/ruby
	puts "It's a test"
	puts 'It\'s also a test'

Output:

	It's a test
	It's also a test

Much better, similar to shell and regex, but unlike shell/regex the backslash still "counts" while inside single quotes.

--------------------------------

Let's do a bit of arithmetic:

	#!/usr/bin/ruby
	puts 10 + 20
	puts 10.00 + 20.00
	puts 4 * 4
	puts 9 / 4
	puts 9 / 4.0
	puts 1 + 3 * 4
	puts (1 + 3) * 4
	puts 3**3

Output:

	30
	30.0
	16
	2
	2.25
	13
	16
	27

Some analysis:

1. As expected
2. I specified 2 decimal places but it only printed one.  If I change it to ``10.01 + 20.01`` it gives an answer of 30.02000000003, weird precision.
3. As expected
4. 9 / 4 = 2 so it seems to round down to the nearest whole number
5. 9 / 4.0 gives the correct answer of 2.25.  I'll have to find out why adding the ".0" increases precision and by how much.
6. As expected, order of operations (multiplication and division before addition and subtraction)
7. As expected, order of operations (parenthesis first)
8. As expected (3 to the 3rd power)

I probably won't need to dig into all this too much because most infrastructure related operations don't get too math-heavy.

--------------------------------

Ruby also has an interactive shell called **irb** that stands for "Interactive RuBy":

	[me@foo ruby]$ irb
	irb(main):001:0>1 + 1
	=> 2
	irb(main):002:0>puts "test test"
	test test
	=> nil
	irb(main):003:0>exit
	[me@foo ruby]$

Neat test environment.

But what's the "nil" thing all about?  Like a return code?  I'll have to dig into that.

--------------------------------

Huh, I just stumbled across "print" and it seems to be the same as "puts" (PUT String).  What are the differences?  Code Academy says...

> The primary difference between them is that puts adds a newline after executing, and print does not.

And they give an example program.  Makes sense!

--------------------------------

That's enough messing around for now, let's move onto something more formalized!