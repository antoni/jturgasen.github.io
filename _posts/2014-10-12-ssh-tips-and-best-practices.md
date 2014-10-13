---
layout: post
title: "SSH Tips and Best Practices"
description: ""
category: 
tags: ["ssh", "linux", "best practices", "AWS"]
---
{% include JB/setup %}

I've collected a huge amount of bookmarks that discuss SSH tips and best practices.  Let's consolidate them here.  First the best practices in no particular order:

* **Protect Private Keys with a Passphrase** - If you don't enter a passphrase when generating your private key it will be stored in plain text.
* **Use ssh-agent to Reduce Typing** - Use ``ssh-agent`` to reduce the number of times you need to type in your private key's passphrase.
* **Generate and Store Private Keys on a Smart Card** - Look for cards that support OpenPGP or use a commercial product like [Crypto Stick](https://www.crypto-stick.com/).
* **Use Protocol v2 Only** - Only allow protocol version 2.  Version is is known to be insecure.
* **Disable Password Authentication** - Only use keys if possible.
* **Create a Banner** - Show your legal disclaimers at each login.
* **Use Two-Factor Authentication** - Google Authenticator has a PAM module to enable TFA for SSH.  Note it does NOT need to hit outside servers to function.
* **Consider gpg-agent for Private Keys** - GPG uses stronger encryption and more passes.
* **Lockdown Using Directives in authorized_keys** - Lots of options in here, look for the AUTHORIZED_KEYS FILE FORMAT section of the man page.
* **Install Keys via Configuration Management Software** - Why push your public keys over and over, let the CM software handle it.  Makes it easy to revoke access, too.
* **Think About Backups** - Are unencrypted private keys being backed up?  If so, who has access to them?
* **HashKnownHosts** - Have SSH save known hosts as hashes rather than plain-text hostnames so the attacker won't know which system(s) to jump to next.
* **Store Host Keys in DNS** - Not a best practice but a neat trick, look at the ``VerifyHostKeyDNS`` option.
* **Only Use the Original EC2 Keypairs as a Backup** - Don't use the keypairs that are generated when an EC2 instance is spun up for day to day access.

And now the tips, tricks, and a couple hardening guides - also in no particular order:

* [SSH Kung Fu](https://www.reddit.com/r/linux/comments/245jt9/ssh_kung_fu/) - Link to an article + Reddit comments with more tips and tricks
* [SSH Tricks: The Unusual and Beyond](http://www.jedi.be/blog/2010/08/27/ssh-tricks-the-usual-and-beyond/) - Pro-tier tricks
* [A sysadmin talks OpenSSH tips and tricks](http://www.tenshu.net/2012/02/sysadmin-talks-openssh-tips-and-tricks.html) - Great stuff once you get past the intro
* [OpenSSH Cookbook: Proxies and Jump Hosts](https://en.wikibooks.org/wiki/OpenSSH/Cookbook/Proxies_and_Jump_Hosts#ProxyCommand_with_Netcat) - Proxies and jump hosts with ``nc`` and SSH
* [Proxies and Jump Hosts](https://news.ycombinator.com/item?id=7973713) - Do you need ``nc`` for jump hosts any more?  Looks like ``-W`` works instead.
* [SSH Keys Workflow](https://www.reddit.com/r/linuxadmin/comments/2hgizs/ssh_keys_workflow/) - Reddit discusses where and how to store SSH keys, some good tips in the comments
* [Restrict Key-based Login Based on Source IP or Hostname](http://blog.tinned-software.net/restrict-ssh-logins-using-ssh-keys-to-a-particular-ip-address/) - Set on a per-key basis, neat trick
* [How to Harden SSH with Identities and Certificates](https://ef.gy/hardening-ssh) - Guide for tinfoil hat level SSH security
* [Security of Automated Access Management Using Secure Shell (SSH) by NIST](http://csrc.nist.gov/publications/PubsDrafts.html#NIST-IR-7966)
* [SSH Config User and Host Aliases](https://news.ycombinator.com/item?id=7973407) - SSH config tricks for logging into system(s) using a different username (vs typing the different username each time)
* [SSH Aliases Support Tab Completion](https://news.ycombinator.com/item?id=7974665) - Works with SFTP too!
* [Simplify your life with an SSH config file](https://news.ycombinator.com/item?id=4677049) - Link to article + comments, via Hacker News