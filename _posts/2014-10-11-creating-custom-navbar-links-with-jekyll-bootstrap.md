---
layout: post
title: "Creating Custom Navbar Links with Jekyll Bootstrap"
description: ""
category: 
tags: ["jekyll bootstrap"]
---
{% include JB/setup %}

Mad props to plusjade for creating [Jekyll Bootstrap](http://jekyllbootstrap.com/).  It makes blogging via Github suuuuuper simple.

After farting around with it this afternoon I figured out how to create custom entries on the navigation bar.  Here's what it looks like now:

![Image not found!](/assets/2014-10-11-my-navbar.png "My Navbar")

Here's a quick step-by-step on how I did it.  We'll use the LinkedIn link as a reference.

First, create a file named ``linkedin.html`` in the root of your blog's directory structure.

Second, open the file and paste in the following:

	---
	layout: page
	title : LinkedIn
	header : LinkedIn
	group: navigation
	---
	{% include JB/setup %}

Next, put in a redirect and a message for the user:

	<meta http-equiv="refresh" content="0; URL=https://www.linkedin.com/in/jturgasen">

	Redirecting to LinkedIn...

That's it, you're done.  Repeat the process for a Twitter link, a link to your resume, or whatever.

Quick and easy!