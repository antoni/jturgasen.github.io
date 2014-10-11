---
layout: post
title: "Why Use VLANs?"
description: ""
category: 
tags: ["networking"]
---
{% include JB/setup %}

I was embarrassed at an interview recently when I couldn't remember many reasons why to use VLANs.  I know that I know but it wasn't on the surface, so I'm going to review.

* **Security** - Unless configured at the router / L3 level VLANs cannot talk to each other.  This allows you to create "secure segments" for things like administrative tasks (public vs private/management/administrative networks) or sensitive data (HIPAA, PCI, classified information, DMZ, etc).
* **Guest Networks** - Many companies have both an internal network and a network for guest Wi-Fi access.  By using VLANs you can put guests into their own segment so they cannot access the internal network.
* **Performance** - Traffic, such as iSCSI or VoIP, is often on it's own VLAN so it does not have to compete with other network traffic and potentially drive down performance.
* **Reduce Broadcast Domains** - When a L2 or L3 broadcast is sent out it does not propagate from one VLAN to another.  This includes ARP and DHCP traffic.  Note that you can configure a DHCP relay if necessary.
* **Virtualize the Network** - Back in the day if you wanted two systems to be on the same LAN segment you needed to plug them into the same switch or switches.  With VLANs you can have two servers that are not in close physical proximity be on the same VLAN and they behave as if they were next to each other (eg: on the same LAN segment).  Note that performance may be reduced due to the hops in between.
* **Remote Administration** - Building on the previous point, a technician no longer has to physically plug a server into a different switch to change it's LAN segment.  Instead, someone can login to the switch remotely and change the VLAN(s) assigned to the port.
* **Tenant Isolation** - In a multi-tenant cloud environment segmentation is crucial.  You can use VLANs to isolate customers from one another.
* **Policy Based Membership** - Newer technologies such as *Dynamic VLANs* allow dynamic membership based on preconfigured rules such as source MAC address or login ID.
* **Reduced Costs** - Instead of having 2 physical NICs, one for each VLAN, you can use a single NIC that acts as a trunk/channel for 2 VLANs.  Not only do you save money by using fewer NICs, but you also need fewer switch ports.

Some other details of note...

* Some switches only support a maximum of 2048 or 4096 VLANs.  This can be a concern in a large cloud environment.
* *VLAN tagging* can be done at different levels depending on your needs (guest OS, host OS, switch port, virtual switch port)
* Inter-switch links (*trunks* or *channels*) must be configured to pass traffic on a per-VLAN basis
* Many switches have a *default VLAN* that allows all hosts connected to the switch to communicate.  This can be good or bad depending on your needs and can be disabled or changed.
* VLANs can be given an administrative names to make configuration easier (eg: no need to remember VLAN 99 is for PCI DSS data, call it the "credit_card" VLAN or something)
* You don't always need a physical (or virtual) router to route traffic between VLANs.  Some switches support both L2 and L3.
* When using OS virtualization the connection between the virtual switch and the physical switch can be configured as a trunk/channel port.  This allows a VM on VLAN 10 and another VM on VLAN 11 to pass traffic over a single physical NIC.