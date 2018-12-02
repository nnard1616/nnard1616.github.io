---
layout: post
title: "MCS 1st Semester Week 10 Notes"
description: My notes for this week's lectures.
date: 2018-11-12
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
        * [10-1 Text Clustering](#10-1-text-clustering)
            * [10-1-1 Motivation](#10-1-1-motivation)
            * [10-1-2 Generative Probabilistic Models Part 1](#10-1-2-generative-probabilistic-models-part-1)
            * [10-1-3 Generative Probabilistic Models Part 2](#10-1-3-generative-probabilistic-models-part-2)
            * [10-1-4 Generative Probabilistic Models Part 3](#10-1-4-generative-probabilistic-models-part-3)
            * [10-1-5 Similiarity Based Approaches](#10-1-5-similiarity-based-approaches)
            * [10-1-6 Evaluation](#10-1-6-evaluation)
        * [10-2 Text Categorization](#10-2-text-categorization)
            * [10-2-1 Motivation](#10-2-1-motivation)
            * [10-2-2 Methods](#10-2-2-methods)
            * [10-2-3 Generative Probabilistic Models](#10-2-3-generative-probabilistic-models)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
        * [Concurrency Control](#concurrency-control)
            * [RPCs](#rpcs)
            * [Transactions](#transactions)
            * [Serial Equivalence](#serial-equivalence)
            * [Pessimistic Concurrency](#pessimistic-concurrency)
            * [Optimistic Concurrency Control](#optimistic-concurrency-control)
        * [Replication Control](#replication-control)
            * [Replication](#replication)
            * [Two-Phase Commit](#two-phase-commit)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)
        * [JUnit](#junit)
            * [Unit Testing](#unit-testing)
            * [Junit Overview](#junit-overview)
            * [JUnit Assertions](#junit-assertions)
            * [JUnit Assumptions](#junit-assumptions)
        * [Parameterized Tests](#parameterized-tests)
            * [Basics](#basics)
            * [Complex Parameters](#complex-parameters)
            * [Writing Parameterized Tests](#writing-parameterized-tests)
            * [Test Generalization](#test-generalization)

<!-- vim-markdown-toc -->


# CS 410 Text Information Systems

---

## Goals and Objectives
* Explain the concept of text clustering and why it is useful.
* Explain how Hierarchical Agglomerative Clustering and k-Means clustering work.
* Explain how to evaluate text clustering.
* Explain the concept of text categorization and why it is useful.
* Explain how Naïve Bayes classifier works.

## Guiding Questions
* What is clustering? What are some applications of clustering in text mining and analysis?
* How does hierarchical agglomerative clustering work? How do single-link, complete-link, and average-link work for computing group similarity? Which of these three ways of computing group similarity is least sensitive to outliers in the data?
* How do we evaluate clustering results?
* What is text categorization? What are some applications of text categorization?
* What does the training data for categorization look like?
* How does the Naïve Bayes classifier work?
* Why do we often use logarithm in the scoring function for Naïve Bayes?

## Additional Readings and Resources
* C. Zhai and S. Massung, Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining. ACM and Morgan & Claypool Publishers, 2016. Chapters 14 & 15.
* Manning, Chris D., Prabhakar Raghavan, and Hinrich Schütze. Introduction to Information Retrieval. Cambridge: Cambridge University Press, 2007. Chapters 13-16.
* Yang, Yiming. An Evaluation of Statistical Approaches to Text Categorization. Inf. Retr. 1, 1-2 (May 1999), 69-90. doi: 10.1023/A:1009982220290

## Key Phrases and Concepts
* Clustering, document clustering, and term clustering
* Clustering bias
* Perspective of similarity
* Hierarchical Agglomerative Clustering, and k-Means
* Direction evaluation (of clustering), indirect evaluation (of clustering)
* Text categorization, topic categorization, sentiment categorization, email routing
* Spam filtering
* Naïve Bayes classifier
* Smoothing

## Video Lecture Notes

### 10-1 Text Clustering

#### 10-1-1 Motivation

![img]({{ '/assets/images/20181112/CS410-wk10-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-009.png' | relative_url }}){: .center-image }

#### 10-1-2 Generative Probabilistic Models Part 1
* differences between mixture and topic model:
    * choice of using a distribution is made once in mixture model , but made multiple times in topic model
    * document clustering has word distribution used to regenerate all the words for a document, But, in the case of one distribution doesn't have to generate all the words in the document. Multiple distribution could have been used to generate the words in the document. 


![img]({{ '/assets/images/20181112/CS410-wk10-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-021.png' | relative_url }}){: .center-image }

#### 10-1-3 Generative Probabilistic Models Part 2

![img]({{ '/assets/images/20181112/CS410-wk10-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-024.png' | relative_url }}){: .center-image }

#### 10-1-4 Generative Probabilistic Models Part 3

![img]({{ '/assets/images/20181112/CS410-wk10-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-032.png' | relative_url }}){: .center-image }

#### 10-1-5 Similiarity Based Approaches

![img]({{ '/assets/images/20181112/CS410-wk10-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-042.png' | relative_url }}){: .center-image }

#### 10-1-6 Evaluation

![img]({{ '/assets/images/20181112/CS410-wk10-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-048.png' | relative_url }}){: .center-image }

### 10-2 Text Categorization

#### 10-2-1 Motivation

![img]({{ '/assets/images/20181112/CS410-wk10-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-052.png' | relative_url }}){: .center-image }

#### 10-2-2 Methods

![img]({{ '/assets/images/20181112/CS410-wk10-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-055.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-056.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-057.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-058.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-059.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-060.png' | relative_url }}){: .center-image }

#### 10-2-3 Generative Probabilistic Models

![img]({{ '/assets/images/20181112/CS410-wk10-img-061.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-062.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-063.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-064.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-065.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-066.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS410-wk10-img-067.png' | relative_url }}){: .center-image }

# CS 425 Distributed Systems

---

## Goals 
* Know how Remote Procedure Calls (RPCs) work.
* Check a run of transactions for correctness (serial equivalence).
* Design systems that use optimistic or pessimistic approaches to ensure correctness in spite of many concurrent clients.
* Detect and avoid deadlocks.
* Calculate nines availability for a replicated system.
* Know how to ensure correctness (consistency) in spite of multiple servers.

## Key Concepts
* LPCs vs RPCs
* Marshaling
* Serial Equivalence
* Pessimistic Concurrency Control
* Optimistic Concurrency Control
* Deadlocks and their detection/avoidance/prevention
* ACID Properties
* Nines Availability
* Active and Passive Replication
* Two-phase commit

## Guiding Questions
* Does an RPC always cross machine boundaries?
* Why is marshaling needed at all?
* What are conflicting operations and how can you use them to detect serial equivalence among transactions?
* Is locking a form of pessimistic or optimistic concurrency control?
* Does Google docs use pessimistic or optimistic concurrency control?
* What is one way to prevent deadlocks among transactions?
* What does “three nines availability” really mean?
* Why is Two-phase commit preferable over One-phase commit?

## Readings and Resources
* There are no readings required for this week.

## Video Lecture Notes

### Concurrency Control

#### RPCs
![img]({{ '/assets/images/20181112/CS425-wk10-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-015.png' | relative_url }}){: .center-image }

#### Transactions
![img]({{ '/assets/images/20181112/CS425-wk10-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-023.png' | relative_url }}){: .center-image }

#### Serial Equivalence
![img]({{ '/assets/images/20181112/CS425-wk10-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-031.png' | relative_url }}){: .center-image }

#### Pessimistic Concurrency
![img]({{ '/assets/images/20181112/CS425-wk10-img-032.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-045.png' | relative_url }}){: .center-image }

#### Optimistic Concurrency Control
![img]({{ '/assets/images/20181112/CS425-wk10-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-052.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-053.png' | relative_url }}){: .center-image }

### Replication Control

#### Replication
![img]({{ '/assets/images/20181112/CS425-wk10-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-055.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-056.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-057.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-058.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-059.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-060.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-061.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-062.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-063.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-064.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS425-wk10-img-065.png' | relative_url }}){: .center-image }

#### Two-Phase Commit

# CS 427 Software Engineering

---

## Goals and Objectives

## Video Lecture Notes

### JUnit

#### Unit Testing
![img]({{ '/assets/images/20181112/CS427-wk10-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-007.png' | relative_url }}){: .center-image }

#### Junit Overview
![img]({{ '/assets/images/20181112/CS427-wk10-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-013.png' | relative_url }}){: .center-image }

#### JUnit Assertions
![img]({{ '/assets/images/20181112/CS427-wk10-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-018.png' | relative_url }}){: .center-image }

#### JUnit Assumptions
![img]({{ '/assets/images/20181112/CS427-wk10-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-023.png' | relative_url }}){: .center-image }

### Parameterized Tests

#### Basics
![img]({{ '/assets/images/20181112/CS427-wk10-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-032.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-033.png' | relative_url }}){: .center-image }

#### Complex Parameters
![img]({{ '/assets/images/20181112/CS427-wk10-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-040.png' | relative_url }}){: .center-image }

#### Writing Parameterized Tests
![img]({{ '/assets/images/20181112/CS427-wk10-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-052.png' | relative_url }}){: .center-image }

#### Test Generalization
![img]({{ '/assets/images/20181112/CS427-wk10-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-055.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-056.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-057.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-058.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-059.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-060.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181112/CS427-wk10-img-061.png' | relative_url }}){: .center-image }

