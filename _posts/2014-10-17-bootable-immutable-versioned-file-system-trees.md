---
layout: post
title: "Bootable, Immutable, Versioned File System Trees"
description: ""
category: 
tags: ["linux", "rhel", "centos"]
---
{% include JB/setup %}

I was digging through my bookmarks tonight and was reminded of [OStree](https://wiki.gnome.org/Projects/OSTree).  These types of architectures are gaining in popularity due to containerization technologies such as Docker.  If you can have all your applications running in containers why have a full OS below the containers?  It has a massive attack surface, patching is a pain, it has way more than you need, and you are wasting disk space.  Lennart Poettering wrote a superb blog entry entitled [Factory Reset, Stateless Systems, Reproducible Systems & Verifiable Systems](http://0pointer.net/blog/projects/stateless.html) where he lays out a ton of use cases for systems similar to OStree.

Are people actually using this stuff?  Yep.  Fedora has [Fedora Atomic / Atomic Cloud Image](https://fedoraproject.org/wiki/Changes/Atomic_Cloud_Image) and Red Hat upstreams it as [Project Atomic (aka RHEL Atomic)](http://www.projectatomic.io/).

It's simple.  It's secure.  It's lightweight.  It makes patching a breeze.  It's the future.