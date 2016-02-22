---
layout: post
title: "Tech Takeaway 037: Maintaining Consistency in a Massively Parallel Environment by Dinah McNutt"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

Sorry for the delay but here's what I've missed along with an early posting for March 2016.

[Maintaining Consistency in a Massively Parallel Environment by Dinah McNutt of Google at USENIX UCMS 2013](https://www.youtube.com/watch?v=_uJlTllziPI)

Takeaways:

* Google has their own package manager (Midas Package Manager / MPM)
* Distribute software using torrents
* Talks a bit about Midas (file signatures, unique hash for the packages, labels, pre/post-install stuff, etc)
* Labels allow things like canary deploys (keyword "canary"), push to live (keyword "live") etc
* Jobs on remote machines pull packages (pull architecture, not push)
* Assumptions and constraints for packaging and distributing software at Google
	* Some systems will be offline
	* Jobs must be able to specify which package version they need
	* Jobs on the same machine may use diff versions of the same package
	* Assurance that files have not been tampered
	* Rollback
* The build system automatically creates the package definition files
* The build system does not create a new package if no files have changed - but it will update the labels on a package if they changed
* They have 3 types of package durability
	* Ephemeral (short-lived, deleted after 2 days by default)
	* Durable (persistent, 3 month-since-last-fetch termination policy)
	* Test (reduced replication and retention policies)
* A "pull" architecture allows developers to choose when they want to pull a new version of the package to the server (vs push on a set schedule)
* Each project chooses how they want to use labels
* Metadata is stored separately from the package
* Package repo failover is built into the client
* Packages are stored on Colossus (distributed file system) and metadata is stored in BigTable
* Talks about how Colossus is fast, reslient, etc but gives no details on why or how
* Google has ACLs for packages / builds
	* Owner (create pkg, delete pkg, change labels)
	* Builder (create pkg, modify labels)
	* Label (change specific labels)
* Labels can be immutable if necessary
* ACLs determine who can decrypt the encrypted files in a package
* Package signatures are created by examining both the package and the metadata (remember, they are separate)
* **mpmdiff** can diff packages and show differences like file ownership/size/pre and post-scripts/checksums
* **File groups** are groups of files within a MPM (eg: a file group for files for an app + a config file)

The title pulled me in and I didn't expect a talk about packaging.  2 meh talks in a row, here's to hoping the next one is better!