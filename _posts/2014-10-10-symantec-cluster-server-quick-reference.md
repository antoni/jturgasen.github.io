---
layout: post
title: "Symantec Cluster Server Quick Reference"
description: ""
category: 
tags: ["veritas"]
---
{% include JB/setup %}

This is my old Veritas Cluster Server (now Symantec Cluster Server) quick reference.

## Commands Quick Reference

* ``hastatus`` - Displays status information about cluster/groups/nodes
* ``hasys`` - Displays information about and administers nodes
* ``hagrp`` - Displays information about and administers service groups
* ``haagent`` - Displays information about and administers agents
* ``hatype`` - Displays information about and administers resource  types
* ``hares`` - Displays information about and administers resources
* ``haclus`` - Displays information about and administers the cluster
* ``gabconfig`` - Displays information about and administers GAB
* ``lltstat`` - Displays information about and administers LLT
* ``hastart`` - Starts the cluster and/or individual nodes
* ``hastop`` - Stops the cluster and/or individual nodes
* ``hacf`` - Checks the integrity of ``main.cf``


## Misc Information
* VCS uses the ``main.cf`` that is in memory, not the one on the disk.  The command ``haconf -dump -makero`` marks the configuration read only and writes it to disk on all nodes.  Keep the configuration read-only all the time unless you need to make changes
* If you must Stop + A a system DO NOT do a ``go`` after.  While the system is halted other Nodes will take over it's Service Groups and typing "go" could lead to data corruption, use ``boot`` instead


## Displaying Information

* ``hastatus`` - Show VCS events in realtime
* ``hastatus -summary`` - Displays the current status of the cluster including the status of service groups, nodes and AutoDisabled service groups
* ``hastatus -group group`` - Displays information about *group* including its resources.  Multiple ``-group group`` options and arguments can be specified

* ``hasys -list`` - Displays a list of all systems
* ``hasys -display`` - Detailed information about all systems
* ``hasys -display system-name`` - Detailed information about *system-name*

* ``hagrp -list`` - Lists all service groups and both their local and global attributes
* ``hagrp -resources group`` - Lists all the resources of *group*
* ``hagrp -dep group`` - Displays the service group dependency tree for *group*
* ``hagrp -display`` - Detailed information about all groups
* ``hagrp -display group`` - Detailed information about *group*
* ``hagrp -display group -sys node`` - Displays the value of all attributes of *group* on *node*
* ``hagrp -display group -attribute attribute`` - Displays the value of *attribute* for group
* ``hagrp -display group -attribute attribute -sys node`` - Same as above but only displays the value of attribute on *node*
* ``hagrp -resources group`` - Displays a list of resources used by *group*

* ``haagent -list`` - List all agents
* ``haagent -display`` - Detailed information about all all ggents
* ``haagent -display agent`` - Detailed information about *agent*

* ``hatype -list`` - Lists all resource types
* ``hatype -display`` - Detailed information about all resource types
* ``hatype -display resource-type`` - Detailed information about *resource-type*

* ``hares -list`` - List all resources and the node each resides on
* ``hares -display`` - Detailed information for all resources including agent arguments and resource status
* ``hares -display resource`` - Same as above but only displays information for *resource*
* ``hares -dep resource`` - Shows dependency tree for *resource*

* ``haclus -display`` - Lists attributes and attribute values for the cluster
* ``haclus -value attribute`` - Displays the value of *attribute*

* ``gabconfig -l`` - Displays the current GAB configuration
* ``gabconfig -a`` - Displays which nodes are running HAD and/or GAB

* ``lltstat`` - Shows counters similar to ``netstat -i``
* ``lltstat -n`` - Displays the Node ID, node name, link state and number of links for each node
* ``lltstat -nvv`` - Displays link names (qfe:0, etc) as well as the MAC address and status of each link on all nodes, the asterisk in the output denotes the node the command was run on
* ``lltstat -c`` - Displays the value of LLT configuration directives on the local bode
* ``lltstat -p`` - Display port status and other information, pretty verbose

* ``lltshow -n node`` - Show the LLT kernel structures on *node*
* ``lltconfig -a list`` - Shows MAC address and interface names for the local node
* ``lltconfig -T query`` - Display heartbeat frequencies

* ``hastart -version`` - Display the version of VCS that's installed


## Starting the Cluster

* If all nodes go down all nodes must be started before the cluster will start.  There are ways around this.
* When all nodes become active they are are all seeded.  Other nodes become seeded if they join later.
* When a node starts up it downloads its configuration (main.cf) from a seeded node
* ``hastart`` must be run on each node in the cluster to start VCS

* ``hastart`` - Starts VCS on the local system
* ``hastart -force`` - Forces VCS to start even if the configuration is marked *stale*.  All nodes that are started after this Node will use configuration from this Node.  Use ``haconf -dump -makero`` after all nodes are started
* ``hastart -onenode`` - Starts VCS on for a single node cluster, DO NOT use this command in a multi-node cluster
* ``hasys -force host`` - Use this if all nodes are in an ADMIN_WAIT state.  *host* is the node that will load its main.cf and push it to all other nodes
* ``hastart -stale`` - Forces a system to wait for another node to have a valid in-memory configuration, then downloads it, and starts (eg: the system treats the on disk configuration as stale)

Use the following steps to start a cluster using a specific configuration file:

1. Copy the configuration file to a single system
2. ``hastop -all -force`` # stops VCS on all Nodes but keeps service groups available
3. ``hastart -stale`` # Use on all nodes except the one that has the new configuration
4. ``hastart`` # Use on the node that has the new configuration

Use the following steps to start a cluster when all nodes are in an **ADMIN_WAIT** state:

1. Find a Node where the on disk main.cf is the one you want to use
2. ``cd /etc/VRTSvcs/config/conf``
3. ``hacf -verify .``
4. ``hastart -force``

Use the following steps to start a cluster when all nodes are in a **STALE_ADMIN_WAIT** state:

1. Find a node where the on disk main.cf is the one you want to use
2. ``hastop -local``
3. ``cd /etc/VRTSvcs/config/conf``
4. ``hacf -verify .``
5. ``hastart -force``

Use the following procedure if all nodes have crashed and one or more nodes cannot be booted (hardware failure, etc)

1. ``hastart`` # run on each remaining node
2. ``gabconfig -cx`` # run on a single node, if this doesn't work see below
3. ``hagrp -online group -sys host`` # starts *group* on *host*, repeat for each service group

Use the following if ``gabconfig -cx`` fails to start the cluster.  Execute each command on every node in the cluster

1. Kill the ``hashadow`` process
2. ``gabconfig -U``
3. ``lltconfig -U`` # Answer "yes"
4. ``lltconfig -c``
5. ``gabconfig -cx`` # seeds the local node
6. ``hastart`` # starts VCS
7. ``hagrp -online group -sys host`` # starts *group* on *host*, repeat for each service group


## Shutting Down The Cluster

Before stopping VCS on node(s) make the configuration read-only with the command ``haconf -dump -makero``!!

* ``hastop -all`` - Stops VCS on all nodes.  Service groups are also stopped
* ``hastop -all -force`` - Stops VCS on all nodes but service groups continue to run

* ``hastop -local -force`` - Stops VCS on the local system, service groups keep running
* ``hastop -force`` - Same as above
* ``hastop -local`` - Stops VCS and all service groups on the local node
* ``hastop -local -evacuate`` - Stops VCS on the local system and moves all service groups to another node

* ``hastop -sys host`` - Stops VCS and all service groups on *host*
* ``hastop -sys host -force`` - Stops VCS on *host* and service groups continue to run
* ``hastop -sys host -evacuate`` - Stops VCS on *host* and evacuates its service groups to another node


## Service Group Administration

Rebooting a system will automatically fail over any Service Groups running on it!

* ``hagrp -switch group -to host`` - Switches *group* to *host*
* ``hagrp -offline group -sys host`` - Offlines *group* that is running on *host*
* ``hagrp -online group -sys host`` - Starts *group* on *host*

* ``hagrp -add group`` - Creates a new Service Group named *group*

* ``hagrp -modify group attribute value1 value2 ...`` - The general syntax for modifying service group attributes
* ``hagrp -modify group SystemList system1 priority system2 priority...`` - Adds the specified systems and their priorities to the **SystemList** for group
* ``hagrp -modify group AutoStart [ 0 | 1 ]`` - Enables or disables auto starting of group, 1 enables and 0 disables
* ``hagrp -modify group AutoStartList system1 system2 ...`` - Adds systems to the auto start list
* ``hagrp -modify group Parallel [ 0 | 1 ]`` - Enables or disables the group as being parallel, you must do this before adding any Resources to the group (ie: right after you create the group)
* ``hagrp -modify group AutoFailOver [ 0 | 1 ]`` - Enables or disables auto failover, if it is disabled and administrator must manually restart the group on another host if it fails
* ``hagrp -modify group FailOverPolicy [ Priority | RoundRobin | Load ]`` - Sets the fail over policy for group, the default is **Priority**
* ``hagrp -modify group SystemZones system1 zone system2 zone ...`` - Sets the system zones for *group*

* ``hagrp -autoenable group -sys host`` - Before doing this ensure the Service Group is offline on all hosts, all of it's Resources are not running outside of VCS control and that there is not a network partition

Use the following steps to freeze and unfreeze a service group.  It will continue to run and VCS will monitor it but take no action on things such as resource faults and concurrency violations.

1. ``haconf -makerw``
2. ``hagrp -freeze group -persistent``
3. ``haconf -dump -makero``
4. At this point the Service Group is frozen, make your changes
5. Once your changes are complete, run the following commands to unfreeze it:
6.  ``haconf -makerw``
7. ``haconf -unfreeze group -persistent``
8. ``haconf -dump -makero``
9. Done!

Use the following steps if a Service Group hangs when it's coming online and you determine it needs to be flushed:

1. Do a ``hastatus -group group | grep Istate`` to see if any resources are hung, replace *group* with the group name
2. If one is hung determine which one it is by using ``hastatus``
3. Ensure the resource is offline at the OS level
4. Use ``hagrp -flush group`` to clear the internal state of *group*
5. Try to start the service group again



## GAB and LLT

* ``gabconfig -c`` - Configures and starts GAB, use this to have a node join an already seeded cluster
* ``gabconfig -c -x`` - Seeds the local node
* ``gabconfig -c -n number`` - Seeds all nodes once *number* of nodes are up and running
* ``gabconfig -b`` - If the GAB client process that requests heartbeats fails to respond to a heartbeat (usually HAD) GAB will panic the system
* ``gabconfig -j`` - When a failed private network link is reconnected systems will panic instead of stopping VCS and GAB
* ``gabconfig -k`` - GAB attempts to kill the GAB client process until it succeeds instead of trying 5 times and then panicing
* ``gabconfig -t milliseconds`` - Configures the GAB timeout value to *milliseconds*, it is recommended you don't change it
* ``gabconfig -u`` - Stops GAB but does not unload the kernel module
* ``gabconfig -s`` - Enables GAB to be used when there is only a single network link for cluster communications, only use this option for testing
* ``gabconfig -f time`` - Time in *miliseconds* to wait for GAB to shut down after an IOFENCE is received, usually after reconnecting a node after a private network failure.  If miliseconds* have elapsed and GAB hasn't shut down the system panics

* ``lltconfig -c`` - Starts LLT and automatically loads the kernel module
* ``lltconfig -U`` - Stops LLT and unloads the kernel module
* ``lltconfig -C id -o`` - Sets the Cluster ID to id
* ``lltconfig -l -d device`` - Indicates device to to be used as a Low Priority Link, device is somtehing like /dev/qfe:0

* ``dlpiping -vs device &`` - Enables the DLPIPing server on device in verbose mode, device is something like /dev/qfe:0
* ``dlpiping -c device remote-MAC-address`` - Sends a ping via LLT, the DLPIPing server must be running on the remote Node.  If this works the Nodes can communicate via LLT

The ``llttest`` command is interactive, unless specified all commands are done within the interactive shell

* ``llttest -p port`` - Has LLT listen on *port*, must be the same on the client and the server
* ``llttest -v`` - Verifies the data in the packets, must be used on both the client and the server
* ``receive -c count`` - Prepares the server to receive *count* number of packets
* ``echo -c count`` - Same as above but sends the packets back to the sender
* ``transmit -c count -n node`` - Sends *count* number of packets to *node*, ensure the node is set to receive them
* ``packet -c count -n node`` - Same as above but the packets are unreliable
* ``debug level`` increases the debug level to *level*


## How to Edit main.cf Directly

1. ``cd /etc/VRTSvcs/conf/config``
2. ``haconf -dump -makero`` # dumps the current cluster configuration to disk
3. ``cp main.cf /tmp/main.cf``
4. ``vi /tmp/main.cf`` # make your changes
5. ``hastop -all -force`` # stops VCS on all nodes but keeps service groups running
6. ``cp /tmp/main.cf /etc/VRTSvcs/conf/config``
7. ``hastart`` # run on all nodes starting with the one you made the changes on


## Starting a Service Group That Won't Start (3 Methods)
Try one of the three following methods to start a service group that won't start on any node.  Manually start the Service Group after using one of the methods below:

1. ``hasys -display`` # see if the Service Group is set to AutoDisabled
2. ``hagrp -autoenable group -sys host`` # Use this command on any Node that the Service Group is **AutoDisabled** on, fill in the *group* and *host* parameters

or...

1. ``hagrp -clear group -sys host`` # clears all faults for the service group on *host*

or...

1. ``hares -clear resource -sys host`` # Fill in *resource* and *host* as appropriate, clears the faulted resource


## Resource Administration

* ``hares -add resource resource-type group`` - Adds *resource* to the service group named *group*.  The resource is type *resource-type*
* ``hares -modify resource attribute value`` - Changes *attribute* for *resource* to *value*
* ``hares -modify resource Critical [ 0 | 1 ]`` - Sets *resource* to non-critical or critical
* ``hares -online resource -sys host`` - Brings resource online on *host*
* ``hares -clear resource`` - Clears faults on *resource*
* ``hares -probe resource -sys host`` - Reprobes *resource* on *host*
* ``hares -link parent child`` - Creates a resource dependency.  *child* must be online for *parent* to start


## Node Administration

* ``hasys -freeze -persistent -evacuate host`` - Freezes *host* and keeps the freeze persistent through reboots
* ``hasys -freeze -evacuate host`` - Same as above but the freeze is not persistent through reboots
* ``hasys -unfreeze -persistent host`` - Unfreezes the persistent freeze on *host*
* ``hasys -unfreeze host`` - Same as above but used when the freeze is non-persistent


## Misc Stuff

* ``hacf -cftocmd /etc/VRTSvcs/config/conf`` - Creates a main.cmd file that contains all of the command lines necessary to recreate the main.cf
* ``haagent -start resource-type -sys host`` - Starts the agent for resource-type on host
* ``hagetcf`` - Creates a TAR file in /tmp that contains the entire VCS directory structure and configuration, useful for making backups, use the -help option for additional usage inforation
* ``/var/adm/VRTSshrd/VRTSlic/bin/vxlicrep`` - Reports on all installed Veritas licenses on the system
* ``licensevcs`` - On the root of the CD-ROM or in the installation tarball, used to install VCS licenses
