---
layout: post
title: "Pets vs Cattle"
description: ""
category: 
tags: ["cloud"]
---
{% include JB/setup %}

You might have heard the term "pets vs cattle" when talking about cloud and next generation architectures.  What does this mean?

**Pets** are unique servers.  That single e-mail server you use, the VM that acts as a centralized hub for scheduled jobs, or that one server that hosts that one application.  Those are pets.  If the died you would have to restore from a backup because rebuilding them from scratch would be almost impossible.  Sure, you *could* rebuild from scratch but odds are it wouldn't be exactly the same.  They are called pets because they are unique and you treat them as special entities.  Another term for this is "server hugging."  I love my unique server and never want it to die!

**Cattle** are servers that are all the same, like a herd of cattle.  In a cloud environment you'll have multiple herds: Your web herd, your app layer herd, and your DB herd.  All of the servers within a herd are the same.  There are no one-off web servers, or that special DB server, or the unique application server.  If one of the cattle dies, no big deal, the herd will live on.

What are the advantages of using cattle instead of pets?  As you will see from the examples below pets are much more inflexible than cattle:

* **Alerting** - Pets need their own, unique monitoring rules.  With cattle you can apply the same monitoring rules to the entire herd.
* **Performance Monitoring** - From a performance standpoint pets need their own performance profile rules.  The hardware may be unique and it probably runs a one-off application that has different performance characteristics.  On the other hand, a herd of cattle has the same hardware profile and runs the same applications so your performance baselines apply to the entire herd.
* **Technical Debt** - Because pets are unique they will create a lot of technical debt over their lifetime.  Need to push something out to all of your VMs?  Hopefully it doesn't break a pet or two.  Need to keep a non-standard version of Apache on a pet?  Now you're managing exception lists which leads to more work.
* **Disaster Recovery** - When one of the cattle dies you can quickly spin up a new VM or clone an existing one.  When a pet dies you will need to either restore from backup or rebuild from scratch.  If you rebuild from scratch will you remember ALL of the unique tweaks and changes that were made over the years?  Probably not.
* **Configuration Management** - Have an environment full of pets?  Have fun writing unique cookbooks (Chef) / manifests (Puppet) for each one.  And, when building the cookbooks/manifests, are you going to remember EVERYTHING that's unique on the server?  With cattle you can write a single cookbook/manifest that applies to the entire herd.
* **Availability / Fault Tolerance** - Pets are, by definition, a single system.  Need four or five nines of uptime for a pet?  You'll have to purchase dual NICs, dual HBAs, use more switch and fibre ports, redundant and/or replicated storage, and 3rd party software like Symantec (Veritas) Cluster Server or VMware High Availability.  With cattle if one of the herd dies the application will keep chugging along, albeit with slightly degraded performance (think RAID 10, 5, or 6).
* **Maintenance** - Building on the previous idea, scheduled maintenance for a pet is a pain in the butt.  Either you must incur downtime or pay extra for a HA solution.  With cattle you can perform your maintenance on one at a time without taking the entire service down.
* **Compliance** - Each pet will need to be evaluated individually for PCI-DSS or HIPAA compliance.  Cattle can be evaluated as a group because they are all the same.
* **Access Controls** - The sysadmins / operations people probably have access to most or all the systems in a company, that's a given.  But who needs access to each pet?  The group that manages the application on a pet will need access to that pet and nothing more.  Same goes for another pet and another.  You can see how this becomes a unwieldy process over time.
* **Firewalls** - Each pet may need it's own unique firewall rules because different applications use different ports, but you can apply the same firewall rules to all members of a herd.
* **Operational Costs** - What do you think costs more when something breaks: Maintaining 10 unique systems or 10 systems that are the same?  I hope the guy who knew everything about a pet documented it before he left the company or got run over by a bus!

You're starting to get the picture.  Because pets are unique it takes a lot of effort and manhours to design, build, and maintain them.  Cattle are all the same so you can re-use code, thus saving a lot of time (and money!).

If you're designing a greenfield environment try to use all cattle, or as many as you can.  Pets with strong DR plans are still viable and even required, but in general aim for more cattle than pets - it will save you a lot of headaches later.