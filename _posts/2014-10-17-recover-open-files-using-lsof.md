---
layout: post
title: "Recover Open Files Using lsof"
description: ""
category: 
tags: ["linux", "rhel", "centos"]
---
{% include JB/setup %}

Whoops, I deleted /var/log/messages by accident.  How do I get it back?  The syslog daemon still has it open [so you can use lsof](http://www.tomhayman.co.uk/linux/lsof/)!

What a sweet trick!