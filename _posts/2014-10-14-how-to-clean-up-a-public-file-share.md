---
layout: post
title: "How to Clean Up a Public File Share"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Ever worked at an organization that has a large, centralized, NAS-type file share?  Often there comes a time when you need to clean it up and/or remove it for compliance reasons.  I had to do this at a previous job so here's some tips:

* **Look at the File Owners** - Start making your way through the files and find out which Windows or UNIX user owns each one and then contact that user.  You can write a script to collect the information for you.  Try to list all of the person's files in the initial e-mail so you don't end up sending them a billion e-mails.
* **What is the File Used For?** - When you talk to a user try to determine who or what uses the file.  An application?  A department?  Is it a shared spreadsheet that people access?  Some files are just old temporary files, some may break a lot of workflows if you remove them.
* **Move the Files and See Who Submits a Ticket** - Sometimes you will find ancient files with no owner that nobody knows anything about.
* **Enable Auditing** - Use some type of auditing to determine who or what is accessing files (OS level auditing, SAMBA logs, FTP logs, etc).
* **Move, Don't Delete Files** - If a file's owner gives the go-ahead to delete a file *move* it to a directory outside the share instead of deleting it.  This way it will seem like it's gone but it's still quickly accessible if they change their mind.
* **Get Operations Involved** - Let them know you are cleaning up the share and that they should send any tickets about the share directly to you.
* **Management Backing** - Don't go crazy, be sure to get management's blessing before you start the cleanup!
* **Make a Final Backup** - Before you finally turn off the share be sure to make one final long-term backup in case anyone needs something important in the future.  Storage is cheap, legal fees and fines are expensive.
* **NFS Mounts / Mapped Drives** - Do you need to clean up other desktops and/or servers that NFS or SMB mount the share?
* **Make the Share Read-only** - Once you are ready to turn off the share (but before your final backup) make it read-only for a few days/weeks to see if any incidents are submitted.  People may ignore your e-mails but they will call in a ticket if they can't write to the share.
* **Create a Folder to Announce Your Intentions** - I created a folder in the root of the share called ``!!! Please do not add new files to this share it is being removed``.  It was at the top of the folder list so anyone accessing the share could see the message.

It's been a while since I did this but I think this is enough to get you started.