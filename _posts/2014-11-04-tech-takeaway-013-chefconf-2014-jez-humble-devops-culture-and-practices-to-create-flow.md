---
layout: post
title: "Tech Takeaway 013: #ChefConf 2014: Jez Humble, DevOps Culture and Practices To Create Flow"
description: ""
category: 
tags: ["chef", "tech takeaway", "devops"]
---
{% include JB/setup %}

[DevOps Culture and Practices To Create Flow by Jez Humble at ChefConf 2014](https://www.youtube.com/watch?v=oX8af9kLhlk&index=7&list=PL11cZfNdwNyMmx0msapJfuGsLV43C7XsA)

* Many DevOps ideas came from Toyota's LEAN ideas
* Talks about testing DR plans
* You should be able to recover from a DR event purely by using version control (Chef cookbooks) and new infrastructure
* The system doesn't know what to do, but it can detect the problem and stop as soon as possible so a human can intervene and fix the problem
* Talks about the "levels" of CI.  The highest level is commit to trunk at least once per day and if automated tests fail fix it in about 10 minutes
* http://www.jamesshore.com/Blog/Continuous-Integration-on-a-Dollar-a-Day.html
* Leaving a build broken is one of the most selfish things you can do
* Great deployment pipeline graphic at 16:34
* Design your deployment pipeline so processes that take the most amount of humans' time (eg: security testing, user acceptance testing) are at the end.  If they are early in the pipeline it ends up costing a lot of manhours
* Consider an environment build (infrastructure build) as part of your pipeline
* "Every build is an RC"
* The purpose of an RC is to prove the build is NOT releasable.  You can't prove the absence of defects, but you can prove there ARE defects
* If you don't feel confident in releasing a build but cannot find defects, it means your validations are not good enough
* How do you speed up your releases?  Talk to people, find out what they are spending their time doing and then optimize based on that
* HP uses a single trunk for all Laserjets and has "feature toggles" so a scanner can start, realize it's a scanner, and turn off unnecessary driver/software/firmware features.  Neat idea
* When a build fails automatically send error logs to the committer
* One large advantage of the agile model is you can respond quickly to changing market conditions, rather than having a long, static roadmap (waterfall)
* LEAN is *NOT* to cut costs.  It's to investing to remove waste.  Removing waste increases throughput.
* Experiments are much cheaper to build that full fledged features.  Give it to the customers.  If it works, improve it.  If not, then not much time and $ is lost
* The flip side is experiments must increase your key metric (eg: revenue).  Collect data, find out if they do.
* "Best practices" are counter-measures to the particular problems they are facing *right now*
* As such, "best practices" change.  The process of that change and adaptation is what's important, not the change itself
* Best practices don't apply directly to you.  Pick and choose which item(s) you need
* Great slide on "high trust culture" at 42:23
* It has to be safe to fail if you want innovation
* The 4 takeaways...
	* Use automation to detect problems quickly
	* Work in small batches
	* Measure and improve customer outcomes
	* Use continuous improvement to get better
* Don't fight stupid, make more awesome