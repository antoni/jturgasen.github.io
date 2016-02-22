---
layout: post
title: "Original Content: What are VXLANs?"
description: ""
category: 
tags: ["networking", "Original Content"]
---
{% include JB/setup %}

What are VXLANs?  Why and when do we use them?  Let's do some research and type it up FAQ style.<BR><BR><BR>

**What are VXLANs?**

Virtual Extensible LANs (VXLANs) encapsulate layer 2 Ethernet frames into layer 3 IP packets and then encapsulates those into layer 4 UDP packets (port 8472).  It allows hosts on two separate L3 networks to appear to be on the same L2 network.  In a way, it abstracts or virtualizes the layer 2 network.  Because it's abstracted, VXLANs can also hide things like duplicate MAC addresses, duplicate IP addresses, and duplicate VLANs.


**What does the encapsulation?**

The VXLAN Tunnel Endpoint (VTEP) does the encapsulation and de-encapsulation.


**What are the components of VXLANs?**

* **Multicast support, IGMP and PIM** - These are required because VXLANs use multicast to communicate
* **VXLAN Network Identifier (VNI)** - 24 bit identifier that specifies the VXLAN network
* **VXLAN Gateway** - Allows the VXLAN to communicate with another network (such as a VLAN or subnet)
* **VXLAN Tunnel End Point (VTEP)** - Performs the encapsulation and de-encapsulation
* **VXLAN Segment / VXLAN Overlay Network** - The segment/network that is identified by the VNI


**How do VXLANs compare to VLANs?**

VLANs are great, but you can only have 4094 maximum according to Cisco.  Perhaps future hardware could greatly increase the limit but it's an expensive solution to a problem we can solve in software.  You can have up to 16 million VXLANs and are not limited by hardware so they present a clear advantage.

The other issue is administration and management.  Sure, we could have two systems in separate data centers and configure them to both be a member of VLAN 999, but it takes a lot of work.  Think about all of the aggregation switches between the two systems that need to be configured!  And what if we're talking about a very dynamic environment like a cloud?  It would be a lot of work to fully automate it.  By using VXLANs we can have a static network and let the VMs / vswitches do the work.

Yet another issue is the limitations of Spanning Tree Protocol (STP) - it tends to fall apart at scale, a single failure can bring down a large chunk of the network, and VLANs can also be viewed as their own fault domains.


**How do VXLANs compare to GRE tunnels?**

VXLANs are when the server (host or guest, physical or virtual) or virtual switch encapsulates the L2 packets into L3 and lets them fly.  With GRE you need to reconfigure the *network* to do the encapsulation.  In a cloud environment it's easy to do this dynamically on the server, but it's harder to dynamically reconfigure the network.  By using VXLANs we can create static routes on the network hardware and let the VMs / vswitches do the work of reconfiguring the network.


**Is VXLAN an open standard?**

Yes!  There is an IETF RFC and commercial vendors worked with the IEEE to create an open standard.


**What types of software support VXLAN?**

vSphere/VMware and [Open vSwitch](https://en.wikipedia.org/wiki/Open_vSwitch) both support it.  Because Open vSwitch supports it we also see it in products such as OpenStack and Red Hat Enterprise Virtualization (RHEV).


**What are some alternatives to using VXLANs?**

Depending on your use case you can create a flat layer 2 network or a "layer 2 tunnel" using [L2TPv3](https://en.wikipedia.org/wiki/L2TPv3), [Ethernet/IP (aka EtherIP)](https://en.wikipedia.org/wiki/EtherNet/IP), or [Generic Routing Encapsulation (GRE)](https://en.wikipedia.org/wiki/Generic_Routing_Encapsulation) tunnels.


**What are the use cases for VXLANs?**

You use VXLANs when you want to create a flat layer 2 network across a layer 3 network.

The other main use case is to use *both* VLANs and VXLANs.  You carve out "virtual wires" (VXLANs) within a single VLAN.


**What are the disadvantages of VXLANs?**

"Traffic tromboning" can happen if you use an IDS or edge device.  For example, let's say you have 2 VMs on the same physical machine and they communicate via VXLAN.  But, what if you require the traffic to go through an IDS / IPS that is on other machine, or at another data center?  Rather than talking directly, the traffic will go out VM1, through the IDS, and back to VM2.  Quite a long route for two VMs that are on the same physical server!

Beware "tunnels in tunnels" such as L2 in L3 in GRE.  It can get out of control quickly.

VXLAN requires multicast, which is often not supported over WAN links.

There are no fault-isolation features as part of it's design.

VXLANs require 1600 byte MTUs (aka Jumbo Frames).  This may not be suitable for all situations.
<BR><BR><BR>

That's enough for now.  I haven't had an opportunity to work with VXLANs but they look fun!


Sources:

* [VXLAN Overview](http://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/white-paper-c11-729383.html) by Cisco.com
* [Typical VXLAN Use Case](http://it20.info/2012/05/typical-vxlan-use-case/) by IT 2.0
* [VXLAN basics and use cases (when / when not to use it)](http://www.yellow-bricks.com/2012/11/02/vxlan-use-cases/) by Yellow Bricks
* [VXLAN Primer-Part 1](http://www.borgcube.com/blogs/2011/11/vxlan-primer-part-1/) by BORGcube
* [VXLAN Primer-Part 2: Let’s Get Physical](http://www.borgcube.com/blogs/2012/03/vxlan-primer-part-2-lets-get-physical/) by BORGcube
* [Digging Deeper into VXLAN, Part 1](http://blogs.cisco.com/datacenter/digging-deeper-into-vxlan/) by Cisco
* [Layer 2 Network is a Single Failure Domain](http://blog.ipspace.net/2012/05/layer-2-network-is-single-failure.html) by Ivan Pepelnjak
* [Examining VXLAN](http://blog.scottlowe.org/2011/12/02/examining-vxlan/) by Scott Lowe