---
layout: post
title: "No More Physical /boot Required in RHEL / CentOS 7"
description: ""
category: 
tags: ["linux", "rhel", "centos", "lvm"]
---
{% include JB/setup %}

Usually when you install RHEL / CentOS you have a separate ``/boot`` partition that's a physical non-LVM partition.  This is because GRUB (aka GRUB Legacy aka GRUB1) could not read LVM volumes.

In RHEL / CentOS 7 they switched to GRUB2 so now there's no need for a separate ``/boot``.

[Source](https://www.reddit.com/r/sysadmin/comments/292qf2/lvm_physical_disk_vs_partitions/cigylwo)