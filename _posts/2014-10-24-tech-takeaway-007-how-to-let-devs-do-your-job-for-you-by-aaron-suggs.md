---
layout: post
title: "Tech Takeaway 007: How to Let Devs Do Your Job For You by Aaron Suggs"
description: ""
category: 
tags: ["tech takeaway", "chef", "ruby"]
---
{% include JB/setup %}

[Aaron Suggs from Kickstarter talks about their environment](https://www.youtube.com/watch?v=K0zd08aECz0&list=UUxEieNpB_tXiUBoF9zkPmAw)

* Use **feature branches** in Git
* Make things frictionless!
* Make local development easy!
* They have a script that kicks off when you clone a repository and installs external dependencies (MySQL, Redis, Ruby, etc)
* Their script uses ``rbenv`` to the right Ruby version
* https://github.com/rtomayko/replicate - replicate locally only certain data (users, DBs, etc) and not ALL data (or it would take forever)
* Just like there are feature branches in Git they use **feature environments**
* Feature environments have web, app, and a snapshot of the prod DB, and it's your own little isolated stack
* Only difference from prod is you have a small number of servers in your feature environment vs many in prod
* Can launch multiple servers in a feature environment if necessary for testing
* Feature environments allow you have multiple, per-developer QA environments.  No wait because someone else is using QA!
* The internal tool they use to create feature environments is called **Hivequeen**
* The tool automatically creates Chef roles and runlists for the environment
* "One button to launch an environment"
* Hivequeen uses the ``fog`` Ruby Gem to talk to the Hosted Chef and AWS APIs
* Remember to loosely couple.  If a component fails make sure the whole site doesn't fail!
* **Dark launch** segments/staggers the rollouts (backend/admin first, the creators, then whole site)
* When someone asks you a question, and you know the answer, show them how to do it instead of doing it for them (DevOps type mentality, let the Devs do it for themselves)
* Blameless postmortems.  The assumption is everyone is acting reasonably, is smart, and competent
* What's your "source of truth"?  Instead of using Knife node searches use EC2 and Chef API calls