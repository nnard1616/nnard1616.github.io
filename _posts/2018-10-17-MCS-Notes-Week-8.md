---
layout: post
title: "MCS 1st Semester Week 8 Notes"
description: My notes for this week's lectures.
date: 2018-10-17
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
        * [8-1 Syntagmatic Relation Discovery](#8-1-syntagmatic-relation-discovery)
            * [8-1-1 Entropy](#8-1-1-entropy)
            * [8-1-2 Conditional Entropy](#8-1-2-conditional-entropy)
            * [8-1-3 Mutual Information Part 1](#8-1-3-mutual-information-part-1)
            * [8-1-4 Mutual Information Part 2](#8-1-4-mutual-information-part-2)
        * [8-2 Topic Mining and Analysis](#8-2-topic-mining-and-analysis)
            * [8-2-1 Motivation and Task Definition](#8-2-1-motivation-and-task-definition)
            * [8-2-2 Term as Topic](#8-2-2-term-as-topic)
            * [8-2-3 Probabalistic Topic Models](#8-2-3-probabalistic-topic-models)
                * [8-2-3-1 Overview of Statistical Language Models Part 1](#8-2-3-1-overview-of-statistical-language-models-part-1)
                * [8-2-3-2 Overview of Statistical Language Models Part 2](#8-2-3-2-overview-of-statistical-language-models-part-2)
                * [8-2-3-3 Mining One Topic](#8-2-3-3-mining-one-topic)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Goals and Objectives](#goals-and-objectives)
    * [Video Lecture Notes](#video-lecture-notes)
        * [5-7 Design Pattern](#5-7-design-pattern)
        * [5-8 Observer Pattern](#5-8-observer-pattern)
        * [5-9 Composite Pattern](#5-9-composite-pattern)

<!-- vim-markdown-toc -->



# CS 410 Text Information Systems

---

(under constructior construction)

## Goals and Objectives
* Explain some basic concepts in natural language processing and text information access.
* Explain why text retrieval is often defined as a ranking problem.
* Explain the basic idea of the vector space retrieval model and how to instantiate it with the simplest bit-vector representation.

## Guiding Questions
* What is entropy? For what kind of random variables does the entropy function reach its minimum and maximum, respectively?
* What is conditional entropy?
* What is the relation between conditional entropy H(X|Y) and entropy H(X)? Which is larger?
* How can conditional entropy be used for discovering syntagmatic relations?
* What is mutual information I(X;Y)? How is it related to entropy H(X) and conditional entropy H(X|Y)?
* What’s the minimum value of I(X;Y)? Is it symmetric?
* For what kind of X and Y, does mutual information I(X;Y) reach its minimum? For a given X, for what Y does I(X;Y) reach its maximum?
* Why is mutual information sometimes more useful for discovering syntagmatic relations than conditional entropy?
* What is a topic?
* How can we define the task of topic mining and analysis computationally? What’s the input? What’s the output?
* How can we heuristically solve the problem of topic mining and analysis by treating a term as a topic? What are the main problems of such an approach?
* What are the benefits of representing a topic by a word distribution?
* What is a statistical language model? What is a unigram language model? How can we compute the probability of a sequence of words given a unigram language model?
* What is Maximum Likelihood estimate of a unigram language model given a text article?
* What is the basic idea of Bayesian estimation? What is a prior distribution? What is a posterior distribution? How are they related with each other? What is Bayes rule?

## Additional Readings and Resources
* C. Zhai and S. Massung. Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining, ACM and Morgan & Claypool Publishers, 2016. Chapters 13, 17.
 
## Key Phrases and Concepts
* Entropy
* Conditional entropy
* Mutual information
* Topic and coverage of topic
* Language model
* Generative model
* Unigram language model
* Word distribution
* Background language model
* Parameters of a probabilistic model
* Likelihood
* Bayes rule
* Maximum likelihood estimation
* Prior and posterior distributions
* Bayesian estimation & inference
* Maximum a posteriori (MAP) estimate
* Prior model
* Posterior mode

## Video Lecture Notes

![img]({{ '/assets/images/20181017/CS410-wk8-img-000.png' | relative_url }}){: .center-image }
### 8-1 Syntagmatic Relation Discovery

#### 8-1-1 Entropy

![img]({{ '/assets/images/20181017/CS410-wk8-img-001.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-002.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-003.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-004.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-005.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-006.png' | relative_url }}){: .center-image }


#### 8-1-2 Conditional Entropy
* Entropy of a word being present is minimum when given exact word is present (ie 0)
* Entropy of a word being present is maximum when given unrelated word is present (ie, H(Xword))

![img]({{ '/assets/images/20181017/CS410-wk8-img-007.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-008.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-009.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-010.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-011.png' | relative_url }}){: .center-image }

#### 8-1-3 Mutual Information Part 1

![img]({{ '/assets/images/20181017/CS410-wk8-img-012.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-013.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-014.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-015.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-016.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-017.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-018.png' | relative_url }}){: .center-image }


#### 8-1-4 Mutual Information Part 2

![img]({{ '/assets/images/20181017/CS410-wk8-img-019.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-020.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-021.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-022.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-023.png' | relative_url }}){: .center-image }



### 8-2 Topic Mining and Analysis


#### 8-2-1 Motivation and Task Definition
![img]({{ '/assets/images/20181017/CS410-wk8-img-024.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-025.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-026.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-027.png' | relative_url }}){: .center-image }


#### 8-2-2 Term as Topic
![img]({{ '/assets/images/20181017/CS410-wk8-img-028.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-029.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-030.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-031.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-032.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-033.png' | relative_url }}){: .center-image }




#### 8-2-3 Probabalistic Topic Models
![img]({{ '/assets/images/20181017/CS410-wk8-img-034.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-035.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-036.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-037.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-038.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-039.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-040.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-041.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-042.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-043.png' | relative_url }}){: .center-image }



##### 8-2-3-1 Overview of Statistical Language Models Part 1
![img]({{ '/assets/images/20181017/CS410-wk8-img-044.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-045.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-046.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-047.png' | relative_url }}){: .center-image }


##### 8-2-3-2 Overview of Statistical Language Models Part 2

![img]({{ '/assets/images/20181017/CS410-wk8-img-048.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-049.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-050.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-051.png' | relative_url }}){: .center-image }


##### 8-2-3-3 Mining One Topic

![img]({{ '/assets/images/20181017/CS410-wk8-img-052.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-053.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-054.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181017/CS410-wk8-img-055.png' | relative_url }}){: .center-image }

# CS 425 Distributed Systems

---

No new content this week, midterm.

# CS 427 Software Engineering

---


## Goals and Objectives
* Design pattern
* Observer pattern
* Composite pattern
* Interpreter Pattern
* Visitor Pattern
* Template Method Pattern
* Iterator Pattern
* Strategy Pattern

## Video Lecture Notes

### 5-7 Design Pattern
* A design pattern is the description of **communicating objects and classes**{:.highlighted} that are customized to solve a general design problem in a particular context, identifying:
    * The participating classes and objects
    * Their roles and collaborations
    * The distribution of responsibilities

* Gang of Four Book: Design Patterns, elements of reusable object-oriented software
    * presents 23 object-oriented patterns:
        * creational patterns (5)
        * structural patterns (7)
        * behavioral patterns (11)

### 5-8 Observer Pattern
* Using an observer pattern can reduce cycles of dependency (one to many dependency)
    * Cycles of dependence
        * If package A depends on package B, then you cannot run the tests for A unless you also have B
            * "A depends on B" means you cannot use A unless you have B
            * "Package A depends on packageB" means that something in A deponds on something in B
        * IF package A depends on package B, then package B should **NOT**{:.highlighted} depend on package A
            * If classes C and D both depend on each other, put them in the same package

### 5-9 Composite Pattern
* Composite
    * problem: 
        * Complex part-whole hierarchy has lots of similar classes
            * Example: book, chaptter, section, paragraph
    * Objectives
        * Simplicity - treat composite (ie, composition of parts) like a part
        * Power - create new kind of part by composing existing ones
        * Safety - treat parts and composites uniformly (no special cases)


![img]({{ '/assets/images/20181017/CS427-wk8-img-1.png' | relative_url }}){: .center-image }

* Behavioral Patterns:
    * Observer: Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
    * Visitor: Represent an operation to be performed on the elements of an object structure.  Visitor lets you define a new operation without changing the classes of the elements on which it operates.
    * Interpreter: Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.
    * Template Method: Define the skeleton of an algorithm in an operation, deferring some steps to subclasses.  Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
    * Iterator: Provide a way to access the elemetns of an aggregate object sequentially without exposing its underlying representation.
    * Strategy: Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

* Structural Patterns
    * Composite: Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

![img]({{ '/assets/images/20181017/CS427-wk8-img-2.png' | relative_url }}){: .center-image }


