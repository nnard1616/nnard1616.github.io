---
layout: post
title: "MCS 2nd Semester Week 1 Notes"
description: My notes for this week's lectures.
date: 2019-01-16
comments: true
tags:
 - UIUC-MCS
---


<!-- vim-markdown-toc Redcarpet -->

* [CS 484 Parallel Programming](#cs-484-parallel-programming)
    * [Lesson 1-1](#lesson-1-1)
        * [Components of a Processor](#components-of-a-processor)
        * [Early History of Computers](#early-history-of-computers)
        * [Moores Law](#moores-law)
        * [End of Moores Law and Parallelism](#end-of-moores-law-and-parallelism)
        * [Peek into the Future](#peek-into-the-future)
    * [Lesson 1-2](#lesson-1-2)
        * [Performance Via Complexity](#performance-via-complexity)
        * [Memory Access Challenges](#memory-access-challenges)
* [CS 498 Cloud Computing Applications](#cs-498-cloud-computing-applications)
    * [](#)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
        * [Lesson 1-1 Cloud Computing Foundations Part 1](#lesson-1-1-cloud-computing-foundations-part-1)
            * [Cloud Computing Introduction](#cloud-computing-introduction)
            * [Motivation Interview](#motivation-interview)
            * [Cloudonomics Part 1](#cloudonomics-part-1)
            * [Cloudonomics Part 2](#cloudonomics-part-2)
            * [Big Data](#big-data)
            * [Summary](#summary)
        * [Lesson 1-2 Cloud Computing Foundations Part 2](#lesson-1-2-cloud-computing-foundations-part-2)
            * [Software Defined Architecture](#software-defined-architecture)
            * [Cloud Services](#cloud-services)
            * [Infrastructure as a Service](#infrastructure-as-a-service)

<!-- vim-markdown-toc -->

# CS 484 Parallel Programming

---

## Lesson 1-1

### Components of a Processor
* To be able to effectively program a modern multiprocessor, we have to understand what it is made up of and how it came to be the way it is today.
* Must have optimal sequential programs before we can use parallel programming
* Need switches that turn on off automatically via voltage application
* Computers are made from some very simple components by putting a large number of them together *in interesting ways*{:.yhighlighted}, and *running them really fast*{:.yhighlighted}.
 
### Early History of Computers
* History be dank yo.

### Moores Law
* In one clock cycle: instruction is fetched from memory, operands fetched, arithmetic performed, results stored back.
 
### End of Moores Law and Parallelism
* Power limit due to temperature, and transistor limit due to close to atomic sizes
* We are in the early days of the Era of Parallel Computing!
### Peek into the Future
* The future is promising for parallel programmers
* Times are changing, ie innovation is becoming stagnant in terms of processors
* Those who can "get" parallel will have an advantage
* If killer parallel app doesn't arrive, progress on single multiprocessor chips will stall
* Complete "stasis" after 7-10-ish years, but then such things have been predicted before.

## Lesson 1-2

### Performance Via Complexity
* obstacles to speed:
    * long chain of gate delays
    * floating point computations
    * slow memory
    * virtual memory and paging
* Overcoming these obstacles can lead to significant increase in complexity, and can make performance difficult to predict and control.
* latency vs throughput and bandwidth
* Programming to avoid branch misprediction
    * when you have data dependent branches that are hard to predict:
        * see if you can convert them into non-branching code!
    * Conditional move instructions help, and normally compilers should do the right thing, but sometimes compilers aren't able to
    * For example:
        * Sum += expression that evaluates to data[c] if >128 or 0 otherwise;
        * Or, since there are only 255 possible values, pre-create a lookup table 
        * Sum += table[data[c]];

### Memory Access Challenges
* Memory is slow, one way to help alleviate this bottleneck is to have intermediary memory devices in between the processor and storage memory (chaches)

# CS 498 Cloud Computing Applications
 
---

## Goals 
* Understand what cloud computing is and why it is important.
* Get a picture of the economics of cloud computing.
* Learn about Big Data (much more about Big Data in the second part of this course).

## Key Concepts
* Cloud computing
* Big Data
* Cloudonmics
* Software defined architecture
* IaaS: Infrastructure as a Service

## Guiding Questions
* What is cloud computing and why is it important?
* Does using cloud computing save my company’s budget?
* What is Big Data?
* How do software defined architectures work?
* What makes virtualizing my computer infrastructure attractive?

## Readings and Resources
None

## Video Lecture Notes

### Lesson 1-1 Cloud Computing Foundations Part 1

#### Cloud Computing Introduction

#### Motivation Interview

#### Cloudonomics Part 1
* U < P / A is when cloud computing makes sense.

#### Cloudonomics Part 2

#### Big Data
* Computers are the necessary tools for fully understanding our physical world!

#### Summary

### Lesson 1-2 Cloud Computing Foundations Part 2

#### Software Defined Architecture
* Saas, PaaS, Iaas are like layers (application layer, operating system layer, data storage layer)

#### Cloud Services

#### Infrastructure as a Service


