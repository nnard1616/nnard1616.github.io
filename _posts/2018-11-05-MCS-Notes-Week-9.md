---
layout: post
title: "MCS 1st Semester Week 9 Notes"
description: My notes for this week's lectures.
date: 2018-11-05
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
        * [9-1-1 The Election Problem](#9-1-1-the-election-problem)
        * [9-1-2 Ring Leader Election](#9-1-2-ring-leader-election)
        * [9-1-3 Election in Chubby](#9-1-3-election-in-chubby)
        * [9-1-4 Bully Algorithm](#9-1-4-bully-algorithm)
        * [9-2-1 Introduction and Basics](#9-2-1-introduction-and-basics)
        * [9-2-2 Distributed Mutual Exclusion](#9-2-2-distributed-mutual-exclusion)
        * [9-2-3 Ricart Agrawalas Algorithm](#9-2-3-ricart-agrawalas-algorithm)
        * [9-2-4 Maekawas Algorithm and WrapUp](#9-2-4-maekawas-algorithm-and-wrapup)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)
        * [9-6-1 Testing Overview](#9-6-1-testing-overview)
        * [9-6-2 Test Cases](#9-6-2-test-cases)
        * [9-6-3 Testing Activities](#9-6-3-testing-activities)
        * [9-6-4 Relating Fault and Failure](#9-6-4-relating-fault-and-failure)
        * [9-6-5 Black Box Testing](#9-6-5-black-box-testing)
        * [9-6-6 White Box Testing](#9-6-6-white-box-testing)
        * [9-6-7 Code Coverage](#9-6-7-code-coverage)

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
* Design Leader Election algorithms including Ring algorithm and Bully algorithm.
* Design Mutual Exclusion algorithms including Ricart-Agrawala’s algorithm and Maekawa’s algorithm.
* Know the design of Leader Election used in industry systems: Google’s Chubby system and Apache Zookeeper.
* Know how industry systems like Google’s Chubby support mutual exclusion.

## Key Concepts
* Google Chubby Leader Election
* Apache Zookeeper Leader Election
* Ring Mutual Exclusion
* Ricart-Agrawala’s Mutual Exclusion
* Maekawa Mutual Exclusion

## Guiding Questions
* What are the safety and liveness conditions for Leader Election?
* Why is Leader Election hard?
* How long does the Ring Election algorithm take to complete?
* How long does the Bully Election algorithm take to complete?
* How does Google Chubby use quorums for election?
* What are the safety and liveness conditions for Mutual Exclusion?
* How do semaphores work?
* How long does the Ring Mutual Exclusion algorithm take to complete?
* How long does the Ricart-Agrawala’s algorithm take to complete?
* Why is Maekawa’s algorithm “optimal”?
* How does Google Chubby use quorums for mutual exclusion?

## Readings and Resources
* (Optional, fun video) [Amazon Web Services (AWS) Training Videos](http://aws.amazon.com/training/intro_series/)

## Video Lecture Notes

### 9-1-1 The Election Problem

![img]({{ '/assets/images/20181105/CS425-wk9-img-1.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-2.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-3.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-4.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-5.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-6.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-7.png' | relative_url }}){: .center-image }

### 9-1-2 Ring Leader Election


![img]({{ '/assets/images/20181105/CS425-wk9-img-8.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-9.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-10.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-11.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-12.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-13.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-14.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-15.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-16.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-17.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-18.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-19.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-20.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-21.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-22.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-23.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-24.png' | relative_url }}){: .center-image }

### 9-1-3 Election in Chubby

![img]({{ '/assets/images/20181105/CS425-wk9-img-25.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-26.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-27.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-28.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-29.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-30.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-31.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-32.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-33.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-34.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-35.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-36.png' | relative_url }}){: .center-image }

### 9-1-4 Bully Algorithm

![img]({{ '/assets/images/20181105/CS425-wk9-img-37.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-38.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-39.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-40.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-41.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-42.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-43.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-44.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-45.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-46.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-47.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-48.png' | relative_url }}){: .center-image }

### 9-2-1 Introduction and Basics

![img]({{ '/assets/images/20181105/CS425-wk9-img-49.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-50.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-51.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-52.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-53.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-54.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-55.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-56.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-57.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-58.png' | relative_url }}){: .center-image }

### 9-2-2 Distributed Mutual Exclusion

![img]({{ '/assets/images/20181105/CS425-wk9-img-59.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-60.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-61.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-62.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-63.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-64.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-65.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-66.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-67.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-68.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-69.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-70.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-71.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-72.png' | relative_url }}){: .center-image }

### 9-2-3 Ricart Agrawalas Algorithm

![img]({{ '/assets/images/20181105/CS425-wk9-img-73.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-74.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-75.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-76.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-77.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-78.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-79.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-80.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-81.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-82.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-83.png' | relative_url }}){: .center-image }

### 9-2-4 Maekawas Algorithm and WrapUp

![img]({{ '/assets/images/20181105/CS425-wk9-img-84.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-85.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-86.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-87.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-88.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-89.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-90.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-91.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-92.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-93.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-94.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-95.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-96.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-97.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-98.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS425-wk9-img-99.png' | relative_url }}){: .center-image }

# CS 427 Software Engineering

---

## Goals and Objectives
* Explain what is validation and verification, respectively.
* Classify a given activity into validation or verification.
* Explain why conduct testing, who write tests, when to write tests, and when to run tests, respectively
* Explain what a test case consists of
* Explain when to use automated tests in contrast to manual tests, and when to use manual tests in contrast to automated tests.
* Explain the purpose of the given kind of tests.
* List the four types of testing activities, and explain their differences
* List the type of knowledge needed by each of the four types of testing activities
* Explain the differences between mistake, fault, erroro, and failure.
* Classify a given case into mistake, fault, error, or failure
* Explain the three conditions illustrated in the PIE model
* Use the PIE model to explain why covering every statement in the software under test is necessary
* Use the PIE model to explain why even covering every statement in the software under test is not sufficient.
* Conduct equivalence partitioning given requirements
* Conduct boundary value analysis given requirements
* Conduct black-box testing given a program under test.
* Explain the type of faults that black-box testing is good at detecting while white-box testing is not.
* Explain the type of faults that white-box testing is good at detecting while black-box testing is not.
* Explain differences between the given major types of code coverage
* Measure code coverage of the given program under test by the given test input values with respect to the given type of code coverage
* Generate test input values to achieve 100% coverage of the given program under test with respect to the given type of code coverage.


## Video Lecture Notes

### 9-6-1 Testing Overview
* Can write tests pretty much at any point in the Waterfall design cycle.

![img]({{ '/assets/images/20181105/CS427-wk9-img-1.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-2.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-3.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-4.png' | relative_url }}){: .center-image }

### 9-6-2 Test Cases

![img]({{ '/assets/images/20181105/CS427-wk9-img-5.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-6.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-7.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-8.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-9.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-10.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-11.png' | relative_url }}){: .center-image }


### 9-6-3 Testing Activities

![img]({{ '/assets/images/20181105/CS427-wk9-img-12.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-13.png' | relative_url }}){: .center-image }

### 9-6-4 Relating Fault and Failure

![img]({{ '/assets/images/20181105/CS427-wk9-img-14.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-15.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-16.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-17.png' | relative_url }}){: .center-image }

* Fault: erroneous piece of code
* Error: an unexpected result/output of code
* Failure: crashes that occur as a result of bad code.
* [see here for paper on PIE...](https://www.cs.odu.edu/~mln/ltrs-pdfs/NASA-92-ieeeswe.jmv.pdf)

### 9-6-5 Black Box Testing

![img]({{ '/assets/images/20181105/CS427-wk9-img-18.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-19.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-20.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-21.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-22.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-23.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-24.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-25.png' | relative_url }}){: .center-image }

### 9-6-6 White Box Testing

![img]({{ '/assets/images/20181105/CS427-wk9-img-26.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-27.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-28.png' | relative_url }}){: .center-image }

### 9-6-7 Code Coverage

![img]({{ '/assets/images/20181105/CS427-wk9-img-29.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-30.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-31.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-32.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-33.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-34.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-35.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-36.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181105/CS427-wk9-img-37.png' | relative_url }}){: .center-image }
