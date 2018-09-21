---
layout: post
title: "MCS 1st Semester Week 4 Notes"
description: My notes for this week's lectures.
date: 2018-09-19
comments: true
tags:
 - UIUC-MCS
---

# CS 410 : Text Information Systems
---
## Goals and Objectives

## Guiding Questions

## Additional Readings and Resources

## Key Phrases and Concepts

## Video Lecture Notes


# CS 425 : Distributed Systems
---

## Goals 
* Know how Napster, Gnutella, FastTrack, and BitTorrent work.
* Know and analyze how distributed hash tables work (Chord, Pastry, and Kelips).

## Key Concepts
* Peer-to-peer systems
* Industrial P2P systems: Napster, Gnutella, FastTrack, BitTorrent
* Distributed hash tables: Chord, Pastry, Kelips

## Guiding Questions
* What is the difference between how Napster clients and Gnutella clients search for files?
* What is the difference between Gnutella and FastTrack?
* What is BitTorrent’s tit for tat mechanism?
* What is consistent hashing?
* Why are DHTs efficient in searching?
* How does Chord route queries?
* How does Pastry route queries?
* How does Kelips route queries?
* What is churn in P2P systems?
* How does Chord maintain correct neighbors in spite of failures and churn?

## Readings and Resources
* [Gnutella v 0.4 paper (PDF)](https://courses.engr.illinois.edu/cs425/fa2014/gnutella_protocol_0.4.pdf)
* [Chord paper (PDF)](http://pdos.csail.mit.edu/papers/chord:sigcomm01/chord_sigcomm.pdf)

## Video Lecture Notes

### 4.1 P2P Systems Introduction
* P2P first distributed systems that seriously focused on scalability with respect to number of nodes
* Widely-deployed P2P Systems
    1.  Napster
    2.  Gnutella
    3.  Fasttrack (Kazaa, Kazaalite, Grokster)
    4.  BitTorrent
* P2P Systems with Provable Properties
    1.  Chord
    2.  Pastry
    3.  Kelips

### 4.2 Napster
![img]({{ '/assets/images/20180919/CS425-wk4-img-1.png' | relative_url }}){: .center-image }

* Napster Operations:
    * Client: Connect to a Napster server
        * Upload list of music files that you want to share
        * Server maintains list of <filename, ip_address, portnum> tuples. **Server stores no files.**{:.highlighted}
    * Search
        * Send server keywords to search with
        * (server searchs its list with the keywords)
        * Server returns a list of hosts -<ip_address, portnum> tuples - to client
        * client pings each host in the list to find transfer rates
        * Client fetches file from best host
    * All communication uses TCP (Transmission control Protocol)
        * Reliable and ordered networking protocol (TCP)

![img]({{ '/assets/images/20180919/CS425-wk4-img-2.png' | relative_url }}){: .center-image }

* Ternary trees store the directory information (3 children per parent)
* Joining a P2P System
    * Can be used for any p2p system
        * Send an http request to well-known url for that p2p service -eg: www.myp2pservice.com
        * Message routed (after lookup in DNS=Domain Name System) to introducer, a well known server that keeps track of some recently joined nodes in p2p system
        * Introducer initializes new peer's neighbor table
* Problems
    * Centralized server a source of congestion
    * Centralized server single point of failure
    * No security: plaintext messages on passwds
    * napster.com declared to be responsible for users' copyright violation
        * "Indirect infringement"
        * events led to Gnutella development

### 4.3 Gnutella
* Gnutella does:
    * eliminate the servers
    * Client machines search and **retrieve**{:.highlighted} amongst themselves (clients act as teh servers)
    * Clients act as servers too, called servents
    * 3/2000 released by AOL, immediately withdrawn due to copyright issues, 88K users by 3/2003
    * Original design underwent several modifications

![img]({{ '/assets/images/20180919/CS425-wk4-img-3.png' | relative_url }}){: .center-image }

* How do I search for my files?
    * Gnutella routes different messages with the overlay graph
    * Protocol has 5 main message types
        * **Query**{:.highlighted} (search)
        * **QueryHit**{:.highlighted} (response to query)
        * **Ping**{:.highlighted} (to probe network for other peers)
        * **Pong**{:.highlighted} (reply to ping, contains address of another peer)
        * **Push**{:.highlighted} (used to initiate file transfer)
    * We'll go into the message structure and protocol now
        * All fields except IP address are in little-endian format
        * Little endian example: 0x12345678 stored as 0x78 in lowest address byte, then 0x56 in next, and so on.

![img]({{ '/assets/images/20180919/CS425-wk4-img-5.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-6.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-7.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-8.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-9.png' | relative_url }}){: .center-image }

* Avoiding Excessive Traffic
    * To avoid duplicate transmissions, each peer maintains a list of recently received messages
    * Query forwarded to all neighbors except peer from which received
    * Each Query (identified by DescriptorID) forwarded only once
    * QueryHit routed back only to peer from which Query received with same DescriptorID
    * For flooded messages, duplicates with same DescriptorID and Payload descriptor are dropped
    * QueryHit with DescriptorID for which Query not seen is dropped.

![img]({{ '/assets/images/20180919/CS425-wk4-img-10.png' | relative_url }}){: .center-image }

* HTTP is file transfer protocol. why?
    * Because it's standard, well-debugged, and widely used.
* Why the "range" field in teh GET request?
    * To support partial file transfers.
* What if responder is behind firewall that disallows incoming connections? (drops it)

![img]({{ '/assets/images/20180919/CS425-wk4-img-11.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-12.png' | relative_url }}){: .center-image }

* Responder establishes a TCP connection at ip_address, port specified.  Sends: GIV <File Index>:<Servent Identifier>/<File Name>\n\n
* Requestor then sends GET to responder (as before) and file is transferred as explained earlier.
* What if requestor is behind firewall too?
    * Gnutella gives up

* If the responding peer is behind a firewall, which of the following statements ARE TRUE about Gnutella?
    * A Push message can be sent to it since it is already connected to its peers.
    * A modified version of Gnutella could use the overlay links themselves to transfer the file (though this may be slow).

* When a Gnutella peer receives a Ping message from one of its neighbors, which of the following actions does it perform? (all are correct) 
    * It forwards it to appropriate neighbors after checking TTL.
    * It creates a Pong message about itself and reverse routes it.
    * If it was the original peer that initiated the Ping, it uses received Pong responses to update its membership lists.
    * It reverse routes any Pong messages it receives.

![img]({{ '/assets/images/20180919/CS425-wk4-img-13.png' | relative_url }}){: .center-image }

* Gnutella Summary
    * No servers
    * Peers/servents maintain "neighbors", this forms an overlay graph
    * Peers store their own files
    * Queries flooded out, ttl restricted
    * QueryHit (replies) reverse path routed
    * Supports file transfer through firewalls
    * Periodic Ping-pong to continuously refresh neighbor lists
        * List size specified by user at peer : heterogeneity means some peers may have more neighbors
        * Gnutella found to follow power law distribution:  P(#Links = L) ~ L<sup>-k</sup>  (k is a constant)
* Problems
    * Ping/Pong constituted 50% traffic
        * Solution: Multiplex, cache and reduce frequency of pings/pongs
    * Repeated searches with same keywords
        * Solution: Cache Query, QueryHit messages
    * Modem-connected hosts do not have enough bandwidth for passing Gnutella traffic
        * Solution: use a central server to act as proxy for such peers
        * Another solution:
            * FastTrack System (soon)
    * Large number of freeloaders
        * 70% of users in 2000 were freeloaders
        * Only download files, never upload own files
    * Flooding causes excessive traffic
        * Is there some way of maintaining meta-information about peers that leads to more intelligent routing?
            --> Structured Peer-to-peer systems, eg Chord System (coming up soon)

### 4.4 FastTrack and BitTorrent

![img]({{ '/assets/images/20180919/CS425-wk4-img-14.png' | relative_url }}){: .center-image }

* FastTrack
    * Hybrid of gnutella and napster
    * Takes advantage of "healthier" participants in the system
    * Underlying technology in Kazaa, KazaaLite, Grokster
    * Proprietary protocaol, but some details available
    * Gnutella, but with some peers designated as **supernodes**{:.highlighted}.
    * A supernode stores a directory listing a subset of nearby (<filename, peer pointer>), similar to Napster servers
    * Supernode membership changes over time **(a member cannot declare itself a supernode)**{:.highlighted}
    * Any peer can become (and stay) a supernode, provided it has earned enough **reputation**{:.highlighted}
        * Kazaalite: participation level (=reputation) of a user between 0 and 1000, initially 10, then affected by length of periods of connectivity and total number of uploads
        * More sophisticated Reputation schemes invented, especially based on economics (See P2PEcon Workshop)
    * A peer searches by contacting nearby supernode.
    * Supernodes get advantage of having information readily available in its structure so it's faster to look up stuff.
    
![img]({{ '/assets/images/20180919/CS425-wk4-img-15.png' | relative_url }}){: .center-image }

* BitTorrent
    * File split into blocks (32 KB - 256 KB)
    * Download **Local Rarest First**{:.highlighted} block policy: prefer early download of blocks that are least replicated among neighbors
        * Exception: New node allowed to pick one random neighbor: helps in bootstrapping
    * **Tit for tat**{:.highlighted} bandwidth usage: Provide blocks to neighbors that provided it the best download rates
        * Incentive for nodes to provide good download rates 
        * Seeds do the same too
    * **Choking**{:.highlighted}: Limit number of neighbors to which concurrent uploads <= a number (eg, 5), ie, the "best" neighbors
        * Everyone else choked
        * Priodically re-evaluate this set (eg, every 10 s)
        * **Optimistic unchoke**{:.highlighted}: periodically (eg, ~30s), unchoke a random neighbor - helps keep unchoked set fresh
    * Choking helps limit how many peers are uploading, to prevent overwhelming upload bandwidth.

    * Why are random choices used in the BitTorrent Choking policy?
        * To avoid the system from getting stuck where only a few peers receive service (correct)
        * To ensure that all peers receive uniform download speed (incorrect)
        * To ensure Tit for Tat bandwidth usage (incorrect)
        
### 4.5 Chord
* DHT: Distributed Hash Table
    * A hash table allows you to insert, lookup and delete objects with keys.
    * A **distributed hash table**{:.highlighted} allows you to do the same in a distributed setting (objects=files)
    * Performance concerns:
        * Load balancing
        * Fault-tolerance
        * Efficiency of lookups and inserts
        * Locality
    * Napster, Gnutella, FastTrack are all DHTs (sort of)
    * So is Chord, a structured peer to peer ssytem that we study next

![img]({{ '/assets/images/20180919/CS425-wk4-img-16.png' | relative_url }}){: .center-image }

* Chord:
    * Developers: I Stoica, D Karger, F Kaashoek, H Balakrishnan, R Morris, Berkeley and MIT
    * Intelligent choice of neighbors to reduce latency and message cost of routing (lookups/inserts)
    * Uses **Consistent Hashing**{:.highlighted} on node's (peer's) address
        * **SHA-1**{:.highlighted}(ip_address.port) --> 160 bit string
        * Truncated to m bits
        * Called peer id (number between 0 and 2<sup>m</sup> - 1)
        * Not unique but ide conflicts very unlikely
        * Can then map peers to one of 2<sup>m</sup> logical points on a circle
    * What are the types of neighbors used in a Chord P2P system (All are correct)?
        * Successors
        * Finger tables
        * Predecessors (if needed)
    * In a Chord P2P system with m=8, a peer with id 33 is considering the following peers for its i=3 finger table entry: 40, 42, and 44. Which one is the best (correct) choice? 
        * 33 + 2^3 = 41, first number that is >= to that is 42


![img]({{ '/assets/images/20180919/CS425-wk4-img-17.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-18.png' | relative_url }}){: .center-image }

    
* what about the files?
    * Filenames also mapped using same consistent hash function
        * SHA-1(filename) --> 160 bit string (key)
        * File is stored at first peer with id greater than or equal to its key (mod 2<sup>m</sup>)
    * File cnn.com/index.html that maps to key K42 is stored at first peer with id greater than 42
        * Note that we are considering a different file-sahring application here: **cooperative web caching**{:.highlighted}
        * The same discussion applies to any other file sharing application, including that of mp3 files.
    * Consistent Hashing => with K keys and N peers, each peer stores O(K/N) keys (ie, < c.K/N, for some constant c) (ie, good load balancing among the peers)
    * In a Chord ring with m=7, three successive peers have ids 12, 19, 33 (there are other peers in the system too, but not in between 12 and 33). If the number of files is large and a uniform hash function is used, which of the following is true?
        * Peer 33 stores about double the number of files as peer 19. 

![img]({{ '/assets/images/20180919/CS425-wk4-img-19.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-20.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-21.png' | relative_url }}){: .center-image }

### 4.6 Failures in Chord
* Search under peer failures
    * Lookup fails (  N16 does not know N45, when N32 fails)
        * Solution:  maintain multiple successors, but how many?
        * Choosing 2log(N) successors suffices to maintain lookup correctness with high probability (ie, ring connected)
            * Say 50% of nodes fail
            * Pr(at given node, at least one successor alive) = 1 - (1/2)^(2logN) = 1 - 1/N^2
            * Pr(above is ttrue at all alive nodes )= (1 - 1/N^2)^(N/2) = e^(-1/2N) ~ 1 
    * Lookup fails ( N45 fails)
        * Solution: replicate file/key at r successors adn predecessors)
* Need to deal with dynamic changes
    * Peers fail (we've discussed this)
    * New peers join
    * Peers leave
        * p2p systems have a high rate of **churn**{:.highlighted} (node join, leave and failure)
            * 25% per hour in Overnet (eDonkey)
            * 100% per hour in gnutella
            * Lower in managed clusters
            * Common feature in all distributed systems, including wide-area (eg PlanetLab), clusters (eg Emulab), clouds (eg AWS) etc
    * So, all the time, need to: --> Need to update successors and fingers, and copy keys

![img]({{ '/assets/images/20180919/CS425-wk4-img-22.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-23.png' | relative_url }}){: .center-image }

* New peers joining:
    * A new peer affects O(log(N)) other finger entries in teh system, on average
    * Number of messages per peer join = O(logN\*logN)
    * Similar set of operations for dealing with peers leaving
        * For dealing with failures, also need **failure detectors**{:.highlighted} (we'll see these later in the course!)

* Stabilization Protocol
    * Concurrent peer joins, leaves, failures might cause loopiness of pointers, and failure of lookups
        * Chord peers periodically run a **stabilization**{:.highlighted} algorithm that checks and updates pointers and keys
        * Ensures *non-loopiness*{:.highlighted} of fingers, eventual success of lookups and O(logN) lookups with high probability
        * Each stabilization round at a peer involves a constant number of messages
        * Strong stability takes O(N^2) stabilization rounds
        * For more see [techReport on Chord webpage]
* Churn
    * When nodes are consttantly joining, leaving, failing
        * Significant effect to consider: traces from the Overnet system show hourly peer turnover rates (churn) could be 25-100% of total number of nodes in system
        * Leads to excessive (unnecessary) key copying (remember that keys are replicated)
        * Stabilization algorithm may need to consume more bandwidth to keep up
        * Main issue is that files are replicated, while it might be sufficient to replicate only meta information about files
        * Alternatives
            * Introduce a level of indirection (any p2p system)
            * Replicate metadata more, eg, Kelips (later in this lecture series)
    * Churn leads to which of the following behaviors in Chord (all are correct)?
        * Successors being continuously updated
        * Finger tables being continuously updated
        * Files being continuously copied to the correct storing servers
<br>

* Virtual nodes
    * Hash can get non-uniform --> Bad load balancing
        * Treat each node as multiple virtual nodes behaving independently 
        * Each joins the system
        * Reduces variance of load imabalance

### 4.7 Pastry
* Pastry
    * Designed by Anthony Rostron (MS Reserach) and Peter Druschel (Rice University)
    * Assigns ids to nodes, just like Chord (using a virtual ring)
    * **Leaf Set**{:.highlighted} Each node knows its successor(s) and predecessor(S)
<br>
* Pastry Neighbors
    * *Routing tables*{:.highlighted} based **<u>prefix matching</u>**{:.highlighted}
        * Think of *hypercube routing*
    * Routing is thus based on prefix matching, and is thus log(N)
        * And hops are short (in the underlying network)
<br>
* Pastry Routing
    * Consider a peer with id 01110100101.  It maintains a neighbor peer with an id matching each of the following prefixes (\* = starting bit differing from this peer's corresponding bit):
        * \*
        * 0\*
        * 01\*
        * 011\*
        * ... 0111010010*
    * When it needs to route to a peer, say 011101**<u>1</u>**{:.highlighted}1001, it starts by forwarding to a neighbor with the largest matching prefix, ie, 011101\*
<br>
* Pastry Locality
    * For each prefix, say 011\*, among all potential neighbors with a matching prefix, the neighbor with the shortest round-trip-time is selected
    * Since shorter prefixes have many more candidates (spread out throughout the Internet), the neighbors for shorter prefixes are likely to be closer than the neighbors for longer prefixes
    * Thus, in the prefix routing, early hops are short and later hops are longer
    * Yet overall "stretch", compared to direc Internet path, stays short.
    * A Pastry peer has two neighbors for prefixes of 101\* and 101110\* respectively. Which of these is more likely to respond to a message faster?
        * The 101\* neighbor
<br>
* Summary of Chord and Pastry
    * Chord and Pastry protocols
        * More structured than Gnutella
        * Black box lookup algorithms
        * Churn handling can get complex
        * O(logN) memory and lookup cost
            * O(logN) lookup hops may be high
            * Can we reduce the number of hops?

### 4.8 Kelips
![img]({{ '/assets/images/20180919/CS425-wk4-img-24.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-25.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-26.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180919/CS425-wk4-img-27.png' | relative_url }}){: .center-image }

* Chord vs Pastry vs Kelips
    * Range of tradeoffs available
        * Memory vs lookup cost vs background bandwidth ( to keep neighbors fresh)

# CS 427 : Software Engineering
---

## Goals and Objectives

## Video Lecture Notes
