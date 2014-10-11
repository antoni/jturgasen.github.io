---
layout: post
title: "My Server is Out of Memory, Which Process is to Blame?"
description: ""
category: 
tags: ["linux troubleshooting", "rhel", "centos", "linux"]
---
{% include JB/setup %}

In the past I've run into issues where a system will run out of memory + swap and grind to a halt.  How do you determine which process is causing the problem?

First, we need to verify that the system is *actually* eating up all of the RAM.  Like [Linux ate my ram!](http://www.linuxatemyram.com/) reminds us, Linux uses unused RAM for file system caching.  If the system is grinding to a halt that means we are out of *both* RAM and swap.  But how do we verify this?

Good old ``sar``!  After we reboot the system we'll use something like like ``sar -S`` to view swap statistics and you will see something like this:

![Image not found!](/assets/2014-10-11-sar-S.png "sar -S")

See the **%swpused** column?  That's the guy we need to keep our eye on.  If the system is truly running out of memory the percent used will increase until it hits 95-100% and the system becomes unresponsive.

So we know we're running out of RAM and swap but how do we determine which process is to blame?  Assuming your monitoring tools don't gather per-process statistics every few minutes there's a very easy way to figure it out!

First, ``ssh`` into the system that's having the problem and start ``screen``.

Next, make a directory to save some output to.  We'll use ``/tmp/test/``.

Next, we are going to create a shell loop that does a "ps" every few seconds and saves the output:

	while true ; do
	  ps -e -ovsz=,args= | sort -b -k1,1n | pr -TW$COLUMNS > /tmp/test/`date +%F--%H-%M-%S`
	  sleep 60
	done

We have created a loop that does a ``ps`` every 60 seconds.  ``ps`` grabs the VSZ (Virtual Memory Size, memory + swap used by a process) and the command, and then formats the output with the largest memory users at the *bottom* of the output.

Let's take a look at one of the output files:

![Image not found!](/assets/2014-10-11-ps-output.png "ps output")

Right now ``gnome-shell`` is the largest memory user.  How does this help us?  Do we know that Gnome is the problem?  Nope!  He's just the biggest one *right now*.  We need to let our loop run and capture more output.  The loop will continue to gather ``ps`` output and save it to text files:

![Image not found!](/assets/2014-10-11-ps-output-files.png "ps output files")

See how we get a new file every 60 seconds?  We're almost done!

Disconnect from your ``ssh / screen`` session and let the loop continue to run *until the system crashes*.

After you reboot it look in ``/tmp/test/`` and you will see lots of text files.  Start looking **at the newest one first** and work your way back in time.  Remember the line at the bottom of each file is the process that's using the most memory.  Usually one will really stick out, it will be *massive* compared to the others.  That's the process who's eating up all your RAM!

There are a few gotchas when doing it this way:

* Sometimes, as the system runs out of memory, you won't get complete ``ps`` output.  If you suspect this is the case continue to look at the files, moving backwards in time, to find the culprit.
* Let the loop keep running while you are disconnected or leave your SSH session open.  The loop needs to keep running to take the ``ps`` snapshots.  How do you it is up to you.
* Before pointing the finger, take a moment to verify your assumption.  If you think the process named xyz is the one that's causing the problem do something like ``cd /tmp/test ; grep xyz *`` and you should see how it grows over time and gets huge.

Hope that helps!  This simple method of looping ``ps`` output has helped me diagnose many runaway processes, everything from application servers to perl scripts.