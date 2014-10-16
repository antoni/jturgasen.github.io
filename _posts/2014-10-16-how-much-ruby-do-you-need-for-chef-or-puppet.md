---
layout: post
title: "How Much Ruby do You Need for Chef or Puppet?"
description: ""
category: 
tags: ["chef", "puppet", "ruby", "configuration management"]
---
{% include JB/setup %}

I've done some research and my conclusion is "not too much unless your company puts pure Ruby code into their cookbooks/manifests."

From a Chef perspective many blog entries say that the [Just Enough Ruby for Chef](https://docs.getchef.com/just_enough_ruby_for_chef.html) cheat sheet is sufficient and [a post from the official Opscode blog](https://www.getchef.com/blog/2013/09/04/demystifying-common-idioms-in-chef-recipes/) seems to confirm it too.  I'm not going to pretend to be a Chef expert but by looking at cookbooks on the Supermarket it seems about right: Variables, booleans, hashes, arrays, conditionals, case statements, a few methods, and that's about it.  From a Puppet perspective [this question](https://docs.puppetlabs.com/guides/faq.html#why-does-puppet-have-its-own-language) in their FAQ addresses the question.

Both Chef and Puppet use what are called **DSLs** or **Domain Specific Languages**.  A DSL is a *mini-language* or a *sub-set* of a language that is specialized to a particular domain.  In this case the domain is Chef, or Puppet, or configuration management.  DSLs provide a level of abstraction so you don't need to know a bunch of pure Ruby to interact with the software.  

So why did I write this silly post?  Because at several interviews I've had people insist that I need to know *actual* Ruby to work with Chef or Puppet.  At the time I didn't have the sources to back up my side argument but now I do!

Bottom line: Unless your company uses pure Ruby in it's cookbooks or manifests you can learn the software itself and not dig deep into actual Ruby.  Don't be intimidated because it uses a Ruby-based DSL, start learning!