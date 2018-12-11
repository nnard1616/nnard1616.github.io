---
layout: post
title: "MCS 1st Semester Week 14 Notes"
description: My notes for this week's lectures.
date: 2018-12-05
comments: true
tags:
 - UIUC-MCS
---


<!-- vim-markdown-toc Redcarpet -->

* [CS 410 Text Information Systems](#cs-410-text-information-systems)
    * [Goals and Objectives](#goals-and-objectives)
    * [Guiding Questions](#guiding-questions)
    * [Additional Readings and Resources](#additional-readings-and-resources)
    * [Key Phrases and Concepts](#key-phrases-and-concepts)
    * [Video Lecture Notes](#video-lecture-notes)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
        * [Security](#security)
            * [Basic Security Concept](#basic-security-concept)
            * [Basic Cryptography Concepts](#basic-cryptography-concepts)
            * [Implementing Mechanism using Cryptography](#implementing-mechanism-using-cryptography)
        * [Datacenter Outage Studies](#datacenter-outage-studies)
            * [What Causes Disasters](#what-causes-disasters)
            * [AWS Outage](#aws-outage)
            * [Facebook Outage](#facebook-outage)
            * [The Planet Outage](#the-planet-outage)
            * [Wrap-up](#wrap-up)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)

<!-- vim-markdown-toc -->


# CS 410 Text Information Systems

---

## Goals and Objectives

## Guiding Questions

## Additional Readings and Resources

## Key Phrases and Concepts

## Video Lecture Notes


# CS 425 Distributed Systems

---

## Goals 
* Know and design security policies.
* Know, apply, and design security mechanisms.
* Know the common properties of various types of networks.
* See case studies of multiple real datacenter outages, and learn lessons from them.

## Key Concepts
* Security policies vs. mechanisms
* Security mechanisms: encryption, authentication, authorization
* Common characteristics among the various types of networks that surround us
* Why do datacenters suffer outages and what can we do about it?

## Guiding Questions
* What is the difference between confidentiality, integrity, and availability?
* What is the difference between authentication, authorization, and auditing?
* What is a replay attack?
* What is the difference between public-private key cryptography and shared key cryptography?
* What is a small world network?
* What is the difference between a small network and a power law network?
* What is the most common cause of datacenter outages?
* What are three techniques to mitigate datacenter outages?

## Readings and Resources
* [Availability Digest Report](200~http://www.availabilitydigest.com/public_articles/0704/data_center_outages-lessons.pdf)
* [AWS Outage Post-mortem](http://aws.amazon.com/message/65648/)
* [Facebook Outage Post-mortem](https://www.facebook.com/notes/facebook-engineering/more-details-on-todays-outage/431441338919)
* [The Planet Outage Post-mortem](http://www.availabilitydigest.com/public_articles/0309/planet_explosion.pdf)

## Video Lecture Notes

### Security

#### Basic Security Concept
![img]({{ '/assets/images/20181205/CS425-wk14-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-007.png' | relative_url }}){: .center-image }

#### Basic Cryptography Concepts
![img]({{ '/assets/images/20181205/CS425-wk14-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-014.png' | relative_url }}){: .center-image }

#### Implementing Mechanism using Cryptography
![img]({{ '/assets/images/20181205/CS425-wk14-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-028.png' | relative_url }}){: .center-image }

### Datacenter Outage Studies

#### What Causes Disasters
![img]({{ '/assets/images/20181205/CS425-wk14-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-032.png' | relative_url }}){: .center-image }

#### AWS Outage
![img]({{ '/assets/images/20181205/CS425-wk14-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-041.png' | relative_url }}){: .center-image }

#### Facebook Outage
![img]({{ '/assets/images/20181205/CS425-wk14-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-047.png' | relative_url }}){: .center-image }

#### The Planet Outage
![img]({{ '/assets/images/20181205/CS425-wk14-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181205/CS425-wk14-img-052.png' | relative_url }}){: .center-image }

#### Wrap-up

# CS 427 Software Engineering

---

## Goals and Objectives

## Video Lecture Notes


