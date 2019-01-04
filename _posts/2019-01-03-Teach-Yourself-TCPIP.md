---
layout: post
title: "Sams Teach Yourself TCP/IP in 24 Hours"
description: My notes from reading this text.
date: 2018-12-05
comments: true
tags:
 - books
 - networking
---


<!-- vim-markdown-toc Redcarpet -->

* [Introduction](#introduction)
* [Part I-TCPIP Basics](#part-i-tcpip-basics)
    * [Hour 1](#hour-1)
        * [Interesting Tidbits](#interesting-tidbits)
        * [Key Terms](#key-terms)
    * [Hour 2](#hour-2)
        * [Interesting Tidbits](#interesting-tidbits)
        * [Key Terms](#key-terms)

<!-- vim-markdown-toc -->

# Introduction

In this blog post I shall record summaries and things of interest that I learn from each chapter (hour) in [Sams Teach Yourself TCP/IP in 24 Hours 4th ed](https://www.amazon.com/Sams-Teach-Yourself-TCP-Hours/dp/0672329964).

# Part I-TCPIP Basics

## Hour 1

### Interesting Tidbits

* Routers (gateways)  help prevent clutter from outside networks on local networks
* A lot of our TCP/IP protocols today are the result of massive efforts from US Military projects (and others)
* Many different protocols for different forms of data transfer.
* Standard -> rules for what's expected
* Implementation -> how the standard is fulfilled

### Key Terms
* **ARPAnet**{:.highlighted}—An experimental network that was the birthplace of TCP/IP.
* **Domain name**{:.highlighted}—An alphanumeric name associated with an IP address
through TCP/IP’s DNS name service system.
* **Gateway**{:.highlighted}—A router that connects a LAN to a larger network. The term
gateway sometimes applies specifically to a router that performs some kind
of protocol conversion.
* **IP address**{:.highlighted}—A logical address used to locate a computer or other networked
device (such as a printer) on a TCP/IP network.
* **Logical address**{:.highlighted}—A network address configured through the protocol software.
* **Name service**{:.highlighted}—A service that associates human-friendly alphanumeric
names with network addresses.
* **Physical address**{:.highlighted}—A permanent address burned into a network adapter in
the factory.
* **Port**{:.highlighted}—An internal address that provides an interface between an application
and TCP/IP’s Transport layer.
* **Protocol system**{:.highlighted}—A system of standards and procedures that enables comput-
ers to communicate over a network.
* **RFC (Request for Comment)**{:.highlighted}—An official technical paper providing relevant
information on TCP/IP or the Internet. You can find the RFCs at several places
on the Internet—try www.rfc-editor.org.
* **Router**{:.highlighted}—A network device that forwards data by logical address and can also
be used to segment large networks into smaller subnetworks.
* **TCP/IP**{:.highlighted}—A network protocol suite used on the Internet and also on many
other networks around the world.



## Hour 2

### Interesting Tidbits
* There are different models for TCP/IP protocols, but they generally have four main layers:
    * Application layer
    * Transport Layer (TCP (reliable but slower) or UDP(faster but less reliable))
    * Internet Layer
    * Network Access Layer
* Others, such as the OSI model, define more precise layers, but in general can roughly be grouped into four main layers listed above.
* As messages travel from one layer to the next, outgoing messages get **encapsulated**{:.highlighted} with header messages (contain info on how to handle message) and incoming messages have their headers unwrapped.
* IP addresses are logical addresses to identify machines on a network (local or internet)

### Key Terms
* **Application layer**{:.highlighted}—The layer of the TCP/IP stack that supports network appli-
cations and provides an interface to the local operating environment.
* **Datagram**{:.highlighted}—The data package passed from the Internet layer to the Network
Access layer, or a data package passed from UDP at the Transport layer to the
Internet layer.
* **Frame**{:.highlighted}—The data package created at the Network Access layer.
* **Header**{:.highlighted}—A bundle of protocol information attached to the data at each layer
of the protocol stack.
* **Internet layer**{:.highlighted}—The layer of the TCP/IP stack that provides logical addressing
and routing.
* **IP (Internet Protocol)**{:.highlighted}—The Internet layer protocol that provides logical
addressing and routing capabilities.
* **Message**{:.highlighted}—In TCP/IP networking, a message is the data package passed from
the Application layer to the Transport layer. The term is also used generically
to describe a message from one entity to another on the network. The term
doesn’t always refer to an Application layer data package.
* **Network Access layer**{:.highlighted}—The layer of the TCP/IP stack that provides an inter-
face with the physical network.
* **Segment**{:.highlighted}—The data package passed from TCP at the Transport layer to the
Internet layer.
* **Application layer**{:.highlighted}—The layer of the TCP/IP stack that supports network appli-
cations and provides an interface to the local operating environment.
* **Datagram**{:.highlighted}—The data package passed from the Internet layer to the Network
Access layer, or a data package passed from UDP at the Transport layer to the
Internet layer.
* **Frame**{:.highlighted}—The data package created at the Network Access layer.
* **Header**{:.highlighted}—A bundle of protocol information attached to the data at each layer
of the protocol stack.
* **Internet layer**{:.highlighted}—The layer of the TCP/IP stack that provides logical addressing
and routing.
* **IP (Internet Protocol)**{:.highlighted}—The Internet layer protocol that provides logical
addressing and routing capabilities.
* **Message**{:.highlighted}—In TCP/IP networking, a message is the data package passed from
the Application layer to the Transport layer. The term is also used generically
to describe a message from one entity to another on the network. The term
doesn’t always refer to an Application layer data package.
* **Network Access layer**{:.highlighted}—The layer of the TCP/IP stack that provides an inter-
face with the physical network.
* **Segment**{:.highlighted}—The data package passed from TCP at the Transport layer to the
Internet layer.





