---
layout: post
title: "What is Partition Alignment and Why Does It Matter?"
description: ""
category: 
tags: ["linux", "rhel", "centos", "storage"]
---
{% include JB/setup %}

Partition alignment is very important for optimal I/O because if the file system is not aligned with the underlying disk a single write will turn into *two* writes.

Let's pretend we have a file system that is not aligned and our application wants to write to file system block #1234.  Block #1234 on the file system maps to *physical* block numbers #9998 and #9999 because the file system is not aligned properly.  We send our write to the file system (block #1234) and it then sends the write to the actual disk which has to write the data to two physical blocks (blocks #9998 and #9999).  See how our single file system write turned into two physical I/Os?

Using a similar example, let's pretend our file system is aligned properly and our application wants to write to file system block #1234.  File system block #1234 maps to physical block #9999.  When our application writes to file system block #1234 the disk stores the data on physical block #9999 - a single I/O.  Compare this to the unaligned file system in the example above where the underlying disk needs to do two physical I/Os.

There are two small disadvantages to proper file system alignment.  The first is you might lose a little bit of space.  If you have a 1 gigabyte disk or LUN you will lose a few bytes or kilobytes to align it properly.  A disadvantage for sure, but it's still worth it to align it properly.  The second disadvantage is automation.  If you're working with a large variety of storage (DAS, SSD, LUNs from different vendors, storage virtualization, etc) the scripting could get very messy.  You will need to evaluate your environment and provisioning processes to find out what works for you.

In short, the advantages of partition alignment greatly outweigh the disadvantages so make sure your partitions are aligned or you might end up doing two physical I/Os for single file system write!


Sources:

* [Linux on 4 KB sector disks: Practical advice](http://www.ibm.com/developerworks/linux/library/l-linux-on-4kb-sector-disks/index.html?ca=drs-) by IBM developerWorks
* [Linux Disk Alignment Reloaded](https://bartsjerps.wordpress.com/2013/03/28/linux-alignment-reloaded/) by Bart Sjerps (EMC)
* [How to align partitions for best performance using parted](http://rainbow.chard.org/2013/01/30/how-to-align-partitions-for-best-performance-using-parted/) by Ian Chard
* [Creating a 7TB Partition Using parted Always Shows "The resulting partition is not properly aligned for best performance"](http://h10025.www1.hp.com/ewfrf/wc/document?cc=uk&lc=en&dlc=en&docname=c03479326) by HP
* [GNU parted: how to deal with the error about proper aligment of partitions](https://superuser.com/questions/349887/gnu-parted-how-to-deal-with-the-error-about-proper-aligment-of-partitions) at SuperUser.com
* [Proper alignment of partitions on an Advanced Format HDD using Parted](http://askubuntu.com/questions/201164/proper-alignment-of-partitions-on-an-advanced-format-hdd-using-parted) at AskUbuntu.com