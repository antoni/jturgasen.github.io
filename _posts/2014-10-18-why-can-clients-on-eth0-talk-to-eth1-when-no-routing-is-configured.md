---
layout: post
title: "Why Can Clients on eth0 Talk to eth1 When No Routing is Configured?"
description: ""
category: 
tags: ["linux", "rhel", "centos", "networking"]
---
{% include JB/setup %}

You have a Linux server with two physical interfaces and you're trying to segment your network.  You want one group of clients to talk to eth0 and another group of clients to talk to eth1.  But, clients on the eth0 segment can talk to eth1.  Why is this and how do I fix it?  Reddit explains.

tl;dr : It's just how it works, use iptables / firewalld to block the traffic.

[Link](https://www.reddit.com/r/linuxadmin/comments/2abbh6/clients_on_eth1_can_connect_to_the_address_of/)