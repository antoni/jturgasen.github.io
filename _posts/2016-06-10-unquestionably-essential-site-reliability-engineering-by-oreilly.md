---
layout: post
title: "Unquestionably Essential: Site Reliability Engineering by O'Reilly"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I'm a man of few words.  Buy.

Some new things I haven't seen in videos, articles, or papers from Google:

* When creating your SLO, talk to your customer, determine their objects, and work towards a reasonable SLO.  This can be better than the usual method of choosing indicators (metrics, alerts) and then creating SLO targets
* When troubleshooting a processing pipeline or wokflow, bisect it and determine which half is broke and work from there.  This can be faster than an end-to-end approach
* To establish a strong software testing culture, require that all bugs be have a matching test created (BDD Bug Driven Development?)
* "Google servers have endpoints that show a sample of RPCs recently sent or received so it's possible to understand how one server is communicating with others without referencing an architecture diagram"
* Make your automated cluster decommisioning workflow idempotent or you might end up wiping a few BigTable clusters by accident

If you like the stuff on my blog you'll love this book so much that you stop reading my blog - promise. 