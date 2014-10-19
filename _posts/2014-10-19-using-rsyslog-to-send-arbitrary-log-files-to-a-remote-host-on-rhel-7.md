---
layout: post
title: "Using rsyslog to Send Arbitrary Log Files to a Remote Host on RHEL / CentOS 7"
description: ""
category: 
tags: ["linux", "rhel", "centos", "logging"]
---
{% include JB/setup %}

Have you ever worked with an application that has it's own log files?  How do you get those logs into syslog on a remote system?  Let's walk through the process.

Note that even though the ``systemd-journal`` is included in RHEL / CentOS 7 the OS [still uses syslog by default](https://securityblog.redhat.com/2014/04/11/new-red-hat-enterprise-linux-7-security-feature-systemd-journald/).

In this example we have two servers:

* ``test1`` is the **client** and has an IP address of 192.168.1.106
* ``test2`` is the **server** and has an IP address of 192.168.1.107

For simplicity's sake let's assume that the client and the server can talk to each other (eg: ``iptables`` / ``firewalld`` / routing / hardware firewall isn't blocking the communications).

First, let's tell our syslog server to listen for incoming connections.  On the server ``test2`` edit ``/etc/rsyslog.conf`` and change

	# Provides TCP syslog reception
	#$ModLoad imtcp
	#$Input TCPServerRun 514

to

	# Provides TCP syslog reception
	$ModLoad imtcp
	$Input TCPServerRun 514

and then bounce ``rsyslog`` to have it re-read the configuration file:

	systemctl restart rsyslog.service

and then verify it's listening it's listening on port 514 TCP

	netstat -lnp | grep -i rsyslog

Next, let's move to the client, ``test1``.  Let's create a little script that generates a fake log file.  I used ``/root/log.sh``

	#!/bin/bash
	
	while true ; do
	  echo 'exception in module blah blah' >> /opt/app/log/applog.log
	  sleep 1
	  echo 'WARNING: Helper process PID 1234 died!' >> /opt/app/log/applog.log
	  sleep 1
	  echo "User 'bob' logged in at 13:13" >> /opt/app/log/applog.log
	  sleep 1
	done
	  
Now create the directory to hold our test logs:

	mkdir -p /opt/app/log

Next we need to configure ``rsyslog`` on the client (``test1``) to monitor our log file.  Edit ``/etc/rsyslog.conf`` and add the following lines:

	## Enable file monitoring
	$ModLoad imfile
	$InputFilePollInterval 10

	## This section monitors the log file /var/app/log/applog.log
	$InputFileName /opt/app/log/applog.log
	$InputFileTag MYAPP
	$InputFileStateFile stat-MYAPP
	$InputFileSeverity debug
	$InputFileFacility local0
	$InputFilePersistStateInterval 20000
	$InputRunFileMonitor

	$template MyApp-format,"%HOSTNAME%", %msg%\n"
	
	if $programname == 'MYAPP'\
	and $syslogfacility-text == 'local0'\
	and $syslogseverity-text == 'debug'\
	then @@192.168.1.107:514;MyApp-format
	& ~

Remember that 192.168.1.107 is the IP of my server (``test2``).  Why did I choose debug instead of info?  Because by default all info messages are put into ``/var/log/messages`` (look at the ``*.info`` line in ``/etc/rsyslogd.conf``) so we would end up with *three* copies (applog.log and syslog on both systems).  Depending on your environment you might want to use a different facility and severity.

Next bounce ``rsyslog`` to have it re-read the configuration file

	systemctl restart rsyslog.service

Now start our log generator script on ``test1`` to have it start spitting out messages

	/root/log.sh

Last, check syslog on ``test2`` (our server) to make sure it's getting the messages:

	tail -f /var/log/messages

You should see our  messages from ``/opt/app/log/applog.log`` on ``host1`` in ``/var/log/messages`` on ``host2``.

One last note: Why do the logs on the server show an IP address and not a FQDN?  [This question on ServerFault](http://serverfault.com/questions/274625/how-do-i-get-rsyslogd-to-log-a-servers-fqdn-instead-of-its-short-hostname) talks about it a bit.

There are tons of other options or ways to do this but hopefully this little primer will get your started.  Thanks for reading!


Sources:

* [imfile on Rsyslog.com](http://www.rsyslog.com/doc/master/configuration/modules/imfile.html)
* [File Monitoring on Loggly](https://www.loggly.com/docs/file-monitoring/)
* [systemd on the Arch Linux wiki](https://wiki.archlinux.org/index.php/systemd)
* [Rsyslog documentation](http://www.rsyslog.com/doc/master/index.html)