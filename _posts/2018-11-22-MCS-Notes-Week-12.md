---
layout: post
title: "MCS 1st Semester Week 12 Notes"
description: My notes for this week's lectures.
date: 2018-11-22
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
        * [12-1 Opinion Mining and Sentiment Analysis](#12-1-opinion-mining-and-sentiment-analysis)
            * [12-1-1 Latent Aspect Rating Analysis Part 1](#12-1-1-latent-aspect-rating-analysis-part-1)
            * [12-1-2 Latent Aspect Rating Analysis Part 2](#12-1-2-latent-aspect-rating-analysis-part-2)
        * [12-2 Text-Based Prediction](#12-2-text-based-prediction)
        * [12-3 Contextual Text Mining](#12-3-contextual-text-mining)
            * [12-3-1 Motivation](#12-3-1-motivation)
            * [12-3-2 Contextual Probabilistic Latent Semantic Analysis](#12-3-2-contextual-probabilistic-latent-semantic-analysis)
            * [12-3-3 Mining topics with social network context](#12-3-3-mining-topics-with-social-network-context)
            * [12-3-4 Mining Causal Topics with Time Series Supervision](#12-3-4-mining-causal-topics-with-time-series-supervision)
        * [12-4 Summary for Exam 2](#12-4-summary-for-exam-2)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
        * [Distributed File Systems](#distributed-file-systems)
            * [File System Abstraction](#file-system-abstraction)
            * [NFS and AFS](#nfs-and-afs)
        * [Distributed Shared Memory](#distributed-shared-memory)
        * [Sensor Networks](#sensor-networks)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)

<!-- vim-markdown-toc -->

# CS 410 Text Information Systems

---

## Goals and Objectives
* Explain why it is necessary and useful to perform joint analysis and mining for text and non-text data.
* Explain the general idea of Contextual Probabilistic Latent Semantic Analysis (CPLSA) and the main difference between CPLSA and PLSA.
* Give multiple application examples of CPLSA for contextual text mining.
* Explain the general idea of using the social network of authors as context to analyze topics in text data and its potential benefit from an application perspective.
* Explain how a time series (such as stock prices) can be used as context to analyze topics in text data that have time stamps using topic models

## Guiding Questions
* Why is text-based prediction interesting from an application perspective? Why are humans playing an important role in text-based prediction? What is the “data mining loop”?
* Why is it necessary and useful to jointly mine and analyze text and non-text data? How can non-text data potentially help in analyzing text data? How can text data potentially help in mining non-text data?
* Can you give some examples of context of a text article? How can we partition text data using context information? Can you give some examples where we can leverage context information to perform interesting comparative analysis of topics in text data?
* What’s the general idea of Contextual Probabilistic Latent Semantic Analysis (CPLSA)? How is it different from PLSA?
* Can you give some examples of interesting topic patterns that can be found by CPLSA? What’s the general idea of using CPLSA for analyzing the impact of an event? Can you think of an interesting application of this kind?
* What’s the general idea of using the social network of authors of text data as a complex context to improve topic analysis for text data? Can you give an example of an interesting application of this kind?
* What’s the general idea of using a time series like stock prices over time to supervise the discovery of topics from text data? Can you give an example of an interesting application of this kind?

## Additional Readings and Resources
* C. Zhai and S. Massung, Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining. ACM and Morgan & Claypool Publishers, 2016. Chapters 18 & 19.
* Hongning Wang, Yue Lu, and ChengXiang Zhai, Latent aspect rating analysis on review text data: a rating regression approach. In Proceedings of ACM KDD 2010, pp. 783-792, 2010. doi: 10.1145/1835804.1835903
* Hongning Wang, Yue Lu, and ChengXiang Zhai. 2011. Latent aspect rating analysis without aspect keyword supervision. In Proceedings of ACM KDD 2011, pp. 618-626. doi: 10.1145/2020408.2020505
* ChengXiang Zhai, Atulya Velivelli, and Bei Yu. A cross-collection mixture model for comparative text mining. In Proceedings of the 10th ACM SIGKDD international conference on knowledge discovery and data mining (KDD 2004). ACM, New York, NY, USA, 743-748. doi: 10.1145/1014052.1014150
* Qiaozhu Mei, Contextual Text Mining, Ph.D. Thesis, University of Illinois at Urbana-Champaign, 2009.
* Hyun Duk Kim, Malu Castellanos, Meichun Hsu, ChengXiang Zhai, Thomas Rietz, and Daniel Diermeier. Mining causal topics in text data: Iterative topic modeling with time series feedback. In Proceedings of the 22nd ACM international conference on information & knowledge management (CIKM 2013). ACM, New York, NY, USA, 885-890. doi: 10.1145/2505515.2505612
* Noah Smith, Text-Driven Forecasting. Retrieved on May 31, 2015 from http://www.cs.cmu.edu/~nasmith/papers/smith.whitepaper10.pdf

## Key Phrases and Concepts
* Text-based prediction
* The “data mining loop”
* Context (of text data) and contextual text mining
* Contextual probabilistic latent semantic analysis (CPLSA): views of a topic and coverage of topics
* Spatiotemporal trends of topics
* Event impact analysis
* Network-regularized topic modeling
* NetPLSA
* Causal topics
* Iterative topic modeling with time series supervision

## Video Lecture Notes

### 12-1 Opinion Mining and Sentiment Analysis

#### 12-1-1 Latent Aspect Rating Analysis Part 1
![img]({{ '/assets/images/20181122/CS410-wk12-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-012.png' | relative_url }}){: .center-image }

#### 12-1-2 Latent Aspect Rating Analysis Part 2
![img]({{ '/assets/images/20181122/CS410-wk12-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-022.png' | relative_url }}){: .center-image }

### 12-2 Text-Based Prediction
![img]({{ '/assets/images/20181122/CS410-wk12-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-026.png' | relative_url }}){: .center-image }

### 12-3 Contextual Text Mining

#### 12-3-1 Motivation
![img]({{ '/assets/images/20181122/CS410-wk12-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-029.png' | relative_url }}){: .center-image }

#### 12-3-2 Contextual Probabilistic Latent Semantic Analysis
![img]({{ '/assets/images/20181122/CS410-wk12-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-032.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-035.png' | relative_url }}){: .center-image }

#### 12-3-3 Mining topics with social network context
![img]({{ '/assets/images/20181122/CS410-wk12-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-041.png' | relative_url }}){: .center-image }

#### 12-3-4 Mining Causal Topics with Time Series Supervision
![img]({{ '/assets/images/20181122/CS410-wk12-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-051.png' | relative_url }}){: .center-image }

### 12-4 Summary for Exam 2
![img]({{ '/assets/images/20181122/CS410-wk12-img-052.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS410-wk12-img-055.png' | relative_url }}){: .center-image }

# CS 425 Distributed Systems

---

## Goals 
* Know the internals of Distributed File Systems like NFS and AFS.
* Know the internals of Distributed Shared Memory systems.
* Know what’s inside a sensor mote and why networks of them are needed.

## Key Concepts
* Distributed File Systems: Why they’re different from single-node file systems
* Internals of NFS
* Internals of AFS
* Distributed Shared Memory: How processes can share memory pages while communicating via messages
* Invalidate protocols in Distributed Shared Memory systems
* Sensor networks: Why they’ve emerged, what’s inside them, where they’re used, and what are the challenges

## Guiding Questions
* Why are Distributed File Systems stateless?
* How does NFS provide transparency?
* Why is whole file caching a reasonable approach in AFS?
* When is invalidate preferable over update in Distributed Shared memory systems?
* Why can’t embedded operating systems be used in sensor motes?
* What is the disadvantage of using a spanning tree in sensor network, for aggregation?

## Readings and Resources
* TinyOS

## Video Lecture Notes

### Distributed File Systems

#### File System Abstraction
![img]({{ '/assets/images/20181122/CS425-wk12-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-014.png' | relative_url }}){: .center-image }

#### NFS and AFS
![img]({{ '/assets/images/20181122/CS425-wk12-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-024.png' | relative_url }}){: .center-image }

### Distributed Shared Memory
![img]({{ '/assets/images/20181122/CS425-wk12-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-032.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181122/CS425-wk12-img-050.png' | relative_url }}){: .center-image }

### Sensor Networks


# CS 427 Software Engineering

---

## Goals and Objectives

## Video Lecture Notes


