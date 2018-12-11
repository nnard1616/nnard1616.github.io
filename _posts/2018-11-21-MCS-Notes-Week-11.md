---
layout: post
title: "MCS 1st Semester Week 11 Notes"
description: My notes for this week's lectures.
date: 2018-11-21
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
        * [11-1 Text Categorization](#11-1-text-categorization)
            * [11-1-1 Discriminative Classifier Part 1](#11-1-1-discriminative-classifier-part-1)
            * [11-1-2 Discriminative Classifier Part 2](#11-1-2-discriminative-classifier-part-2)
            * [11-1-3 Evaluation Part 1](#11-1-3-evaluation-part-1)
            * [11-1-4 Evaluation Part 2](#11-1-4-evaluation-part-2)
        * [11-2 Opinion Mining and Sentiment Analysis](#11-2-opinion-mining-and-sentiment-analysis)
            * [11-2-1 Motivation](#11-2-1-motivation)
            * [11-2-2 Sentiment Classification](#11-2-2-sentiment-classification)
            * [11-2-3 Ordinal Logistic Regression](#11-2-3-ordinal-logistic-regression)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
        * [Stream Processing](#stream-processing)
        * [Distributed Graph Processing](#distributed-graph-processing)
        * [Structure of Networks](#structure-of-networks)
        * [Scheduling](#scheduling)
            * [Single-processor Scheduling](#single-processor-scheduling)
            * [Hadoop Scheduling](#hadoop-scheduling)
            * [Dominant-resource Fair Scheduling](#dominant-resource-fair-scheduling)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)

<!-- vim-markdown-toc -->



# CS 410 Text Information Systems

---

## Goals and Objectives
* Explain the basic ideas of Logistic Regression, K-Nearest Neighbors (k-NN), and how K-NN works.
* Explain how to evaluate categorization results.
* Explain the tasks of opinion mining and sentiment analysis and why they are important tasks from an application perspective.
* Explain how sentiment analysis can be done using text categorization techniques and why a straightforward application of regular text categorization techniques may not be adequate.
* Give examples of both simple and complex features that are used for characterizing text data and explain how NLP can enable complex features to be generated from text.

## Guiding Questions
* What’s the general idea of the logistic regression classifier? How is it related to Naïve Bayes? Under what conditions would logistic regression cover Naïve Bayes as a special case for two-category categorization?
* What’s the general idea of the k-Nearest Neighbor classifier? How does it work?
* How do we evaluate categorization results?
* How do we compute classification accuracy, precision, recall, and F score?
* Why is harmonic mean as used in F better than the arithmetic mean of precision and recall?
* What’s the difference between macro and micro averaging?
* Why is it sometimes interesting to frame a categorization problem as a ranking problem?
* What is an opinion? How is it different from a factual statement?
* What’s an opinion holder? What’s an opinion target?
* What’s the goal of opinion mining?
* What is sentiment analysis? How is it similar to and different from a text categorization task such as topic categorization?
* Why are unigram features generally insufficient for accurate sentiment classification?
* What’s the concern of using too many complex features such as frequent substructures of parse trees?
* What are some commonly used features to represent text data?

## Additional Readings and Resources
* C. Zhai and S. Massung, Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining. ACM and Morgan & Claypool Publishers, 2016. Chapters 15 & 18.
* Yang, Yiming. An Evaluation of Statistical Approaches to Text Categorization. Inf. Retr. 1, 1-2 (May 1999), 69-90. doi: 10.1023/A:1009982220290
* Bing Liu, Sentiment analysis and opinion mining. Morgan & Claypool Publishers, 2012.
* Bo Pang and Lillian Lee, Opinion mining and sentiment analysis, Foundations and Trends in Information Retrieval 2(1-2), pp. 1–135, 2008.

## Key Phrases and Concepts
* Generative classifier vs. discriminative classifier
* Training data
* Logistic regression
* K-Nearest Neighbor classifier
* Classification accuracy, precision, recall, F measure, macro-averaging, and micro-averaging
* Opinion holder, opinion target, sentiment, and opinion representation
* Sentiment classification
* Features, n-grams, frequent patterns, and overfitting

## Video Lecture Notes

### 11-1 Text Categorization

#### 11-1-1 Discriminative Classifier Part 1
![img]({{ '/assets/images/20181121/CS410-wk11-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-008.png' | relative_url }}){: .center-image }

#### 11-1-2 Discriminative Classifier Part 2
![img]({{ '/assets/images/20181121/CS410-wk11-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-021.png' | relative_url }}){: .center-image }

#### 11-1-3 Evaluation Part 1
![img]({{ '/assets/images/20181121/CS410-wk11-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-032.png' | relative_url }}){: .center-image }

#### 11-1-4 Evaluation Part 2
![img]({{ '/assets/images/20181121/CS410-wk11-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-038.png' | relative_url }}){: .center-image }

### 11-2 Opinion Mining and Sentiment Analysis

#### 11-2-1 Motivation
![img]({{ '/assets/images/20181121/CS410-wk11-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-050.png' | relative_url }}){: .center-image }

#### 11-2-2 Sentiment Classification
![img]({{ '/assets/images/20181121/CS410-wk11-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-052.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-055.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-056.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-057.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-058.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-059.png' | relative_url }}){: .center-image }

#### 11-2-3 Ordinal Logistic Regression
![img]({{ '/assets/images/20181121/CS410-wk11-img-060.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-061.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-062.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-063.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-064.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-065.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS410-wk11-img-066.png' | relative_url }}){: .center-image }

# CS 425 Distributed Systems

---

## Goals 
* Apply classical scheduling algorithms.
* Apply popular Hadoop scheduling algorithms.
* Know the internals of Apache Storm, a stream processing engine.
* Know how enormous graphs can be processed in clouds.

## Key Concepts
* Classical Scheduling algorithms, including FIFO, Shortest Task First, and Round Robin
* Popular Hadoop schedulers including Capacity Scheduler and Fair Scheduler
* Internals of Apache Storm, a stream processing engine
* Internals of distributed graph processing engines, e.g., Google’s Pregel

## Guiding Questions
* Why is shortest-task-first optimal?
* What is the difference between the Capacity and Fair schedulers in Hadoop?
* What is a Storm topology?
* What is Gather-Apply-Scatter paradigm in distributed graph processing?

## Readings and Resources
[Apache Storm](http://storm.apache.org/)
[Optional: Spark Streaming](http://www.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
[Pregel](http://dl.acm.org/citation.cfm?id=1807184)

## Video Lecture Notes

### Stream Processing
![img]({{ '/assets/images/20181121/CS425-wk11-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-006.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-011.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-017.png' | relative_url }}){: .center-image }

### Distributed Graph Processing
![img]({{ '/assets/images/20181121/CS425-wk11-img-018.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-023.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-027.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-032.png' | relative_url }}){: .center-image }

### Structure of Networks
![img]({{ '/assets/images/20181121/CS425-wk11-img-033.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-043.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-046.png' | relative_url }}){: .center-image }

### Scheduling

#### Single-processor Scheduling
![img]({{ '/assets/images/20181121/CS425-wk11-img-047.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-051.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-052.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-055.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-056.png' | relative_url }}){: .center-image }

#### Hadoop Scheduling
![img]({{ '/assets/images/20181121/CS425-wk11-img-057.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-058.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-059.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-060.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-061.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-062.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-063.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-064.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-065.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-066.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-067.png' | relative_url }}){: .center-image }

#### Dominant-resource Fair Scheduling
![img]({{ '/assets/images/20181121/CS425-wk11-img-068.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-069.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-070.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-071.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-072.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-073.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-074.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-075.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-076.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181121/CS425-wk11-img-077.png' | relative_url }}){: .center-image }

# CS 427 Software Engineering

---

## Goals and Objectives

## Video Lecture Notes


