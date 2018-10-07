---
layout: post
title: "MCS 1st Semester Week 6 Notes"
description: My notes for this week's lectures.
date: 2018-10-07
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
        * [6-1 Learning to Rank Part 1](#6-1-learning-to-rank-part-1)
        * [6-2 Learning to Rank Part 2](#6-2-learning-to-rank-part-2)
        * [6-3 Learning to Rank Part 3](#6-3-learning-to-rank-part-3)
        * [6-4 Future of Web Search](#6-4-future-of-web-search)
        * [6-5 Recommender Systems-Content-Based Filtering Part 1](#6-5-recommender-systems-content-based-filtering-part-1)
        * [6-6 Recommender Systems-Content-Based Filtering Part 2](#6-6-recommender-systems-content-based-filtering-part-2)
        * [6-7 Recommender Systems-Collaborative Filtering Part 1](#6-7-recommender-systems-collaborative-filtering-part-1)
        * [6-8 Recommender Systems-Collaborative Filtering Part 2](#6-8-recommender-systems-collaborative-filtering-part-2)
        * [6-9 Recommender Systems-Collaborative Filtering Part 3](#6-9-recommender-systems-collaborative-filtering-part-3)
* [CS 425 Distributed Systems](#cs-425-distributed-systems)
    * [Goals](#goals)
    * [Key Concepts](#key-concepts)
    * [Guiding Questions](#guiding-questions)
    * [Readings and Resources](#readings-and-resources)
    * [Video Lecture Notes](#video-lecture-notes)
* [CS 427 Software Engineering](#cs-427-software-engineering)
    * [Video Lecture Notes](#video-lecture-notes)
        * [5-1 Object-Oriented Modeling](#5-1-object-oriented-modeling)
        * [5-2 Class Diagram-Overview](#5-2-class-diagram-overview)
        * [5-3 Class Diagram-Relationships](#5-3-class-diagram-relationships)
        * [5-4 Class Diagram-Miscellaneous](#5-4-class-diagram-miscellaneous)
        * [5-5 Requirements to Class Diagram](#5-5-requirements-to-class-diagram)
        * [5-6 Sequence Diagram](#5-6-sequence-diagram)

<!-- vim-markdown-toc -->
# CS 410 Text Information Systems

---

## Goals and Objectives
* Explain how we can extend a retrieval system to perform content-based information filtering (recommendation).
* Explain how we can use a linear utility function to evaluate an information filtering system.
* Explain the basic idea of collaborative filtering.
* Explain how the memory-based collaborative filtering algorithm works.

## Guiding Questions
* What is content-based information filtering?
* How can we use a linear utility function to evaluate a filtering system? How should we set the coefficients in such a linear utility function?
* How can we extend a retrieval system to perform content-based information filtering?
* What is the exploration-exploitation tradeoff?
* How does the beta-gamma threshold learning algorithm work?
* What is the basic idea of collaborative filtering?
* How does the memory-based collaborative filtering algorithm work?
* What is the “cold start” problem in collaborative filtering?

## Additional Readings and Resources
* C. Zhai and S. Massung. Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining, ACM Book Series, Morgan & Claypool Publishers, 2016. Chapters 10 - Section 10.4,Chapters 11

## Key Phrases and Concepts
* Content-based filtering
* Collaborative filtering
* Beta-gamma threshold learning
* Linear utility
* User profile
* Exploration-exploitation tradeoff
* Memory-based collaborative filtering
* Cold start

## Video Lecture Notes

### 6-1 Learning to Rank Part 1
* How can we combine many features?  (learning to rank)
    * General Idea:
        * Given a query-doc pair (Q,D), define various kinds of features Xi(Q,D)
        * Examples of feature: the number of overlapping terms, BM25 score of Q and D, p(Q\|D), PageRank of D, p(Q\|Di), where Di may be anchor text or big font text, "does the URL contain '~'?"...
        * Hypothesize p(R=1\|Q,D)=s(X1(Q,D)),...,Xn(Q,D),&lambda;) where &lambda; is a set of parameters
        * Learn &lambda; by fitting functions with training data, ie, 3-tuples like (D,Q,1)(Dis relevant to Q) or (D,Q,0)(D is non-relevant to Q)

### 6-2 Learning to Rank Part 2

![img]({{ '/assets/images/20181007/CS410-wk6-img-1.png' | relative_url }}){: .center-image }

### 6-3 Learning to Rank Part 3
* More Advanced Learning Algorithms
    * Attempt to directly optimize a retrieval measure (eg MAP, nDCG)
        * More difficult as an optimization problem
        * Many solutions were proposed 
    * Can be applied to many other ranking problems beyond search
        * Recommender systems
        * Computational advertising
        * Summarization

* Summary
    * Machine Learning has been applied to text retrieval since many decades ago (eg, Rocchio feedback)
    * Recent use of machine learning is driven by 
        * large-scale training data available
        * need for combining many features
        * need for robust ranking (again spams)
    * Modern Web search engines all use some kind of ML technique to combine many features to optimize ranking
    * Learning to ranki is still an active research topic

### 6-4 Future of Web Search
* Next generation search engines
    * more specialized/customized (vertical search engines)
        * special group of users (community engines, eg, Citeseer)
        * Personalized (better understanding of users)
        * Special genre/domain (better understanding of documents)
    * Learning over time (evolving)
    * Integration of search, navigation, and recommendation/filtering (full-fledged information management)
    * Beyond search to support tasks (eg shopping)
    * Many opportunities for innovations!

![img]({{ '/assets/images/20181007/CS410-wk6-img-2.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS410-wk6-img-3.png' | relative_url }}){: .center-image }

* Above image shows what current search engines do (search, keyword queries, bag of words), and what people are working to expand it to do.

![img]({{ '/assets/images/20181007/CS410-wk6-img-4.png' | relative_url }}){: .center-image }

### 6-5 Recommender Systems-Content-Based Filtering Part 1
* Two modes of text access: Pull vs Push
    * Pull mode (search engines)
        * users take initiative
        * Ad hoc information need
    * Push mode (recommender systems)
        * systems take initiative
        * stable information need or system has good knowledge about a user's need

* Recommender &asymp; Filtering system
    * Stable & long term interest, dynamic info source
    * System must make a delivery decision immediately as a document "arrives"

![img]({{ '/assets/images/20181007/CS410-wk6-img-5.png' | relative_url }}){: .center-image }

* Basic Filtering Question: Will User U Like Item X?
    * Two different ways of answering it
        * look at what items U likes, and then check if X is similar
            * Item similarity => content-based filtering
        * Look at who likes X, and then check if U is similar
            * User similarity => collaborative filtering
    * Can be combined

![img]({{ '/assets/images/20181007/CS410-wk6-img-6.png' | relative_url }}){: .center-image }

* Three Basic Problems in content-based filtering
    * making **filtering decision**{:.highlighted} (binary classifier)
        * Doc text, profile text --> yes/no
    * **Initialization**{:.highlighted}
        * initialize the filter based on only the profile text or very few examples
    * **Learning**{:.highlighted} from 
        * Limited relevance judegements (only on "yes" docs)
        * Accumulated documents
    * All trying to maximize the utility

* Extend a retrieval system for information filtering
    * "Reuse" retrieval techniques to score documents
    * use a score threshold for filtering decision
    * learn to improve scoring with traditional feedback
    * new approaches to threshold setting and learning

![img]({{ '/assets/images/20181007/CS410-wk6-img-7.png' | relative_url }}){: .center-image }

### 6-6 Recommender Systems-Content-Based Filtering Part 2

![img]({{ '/assets/images/20181007/CS410-wk6-img-8.png' | relative_url }}){: .center-image }

* Empirical utility optimization
    * Basic idea
        * compute the utility on the training data for each candidate score threshold
        * choose the threshold that gives the maximum utility on the training data set
    * difficulty: biased training sample!
        * we can only get an upper bound for the true optimal threshold
        * could a discarded item be possibly interesing to the user?
    * solution:
        * heuristic adjustment (lowering) of threshold

![img]({{ '/assets/images/20181007/CS410-wk6-img-9.png' | relative_url }}){: .center-image }

* Beta-Gamma thereshold learning
    * Pros 
        * explicitly addresses exploration-exploitation tradeoff ("safe" exploration)
        * Arbitrary utility (with appropriate lower bound)
        * empirically effective
    * Cons
        * Purely heuristic
        * Zero utility lower bound often too conservative

* Summary
    * Two strategies for recommendation/filtering
        * content-based (item similarity)
        * collaborative filtering (user similarity)
    * Content-based recommender system can be built based on a search engine system by 
        * adding threshold mechanism
        * adding adaptive learning algorithms

### 6-7 Recommender Systems-Collaborative Filtering Part 1
* What is Collaborative filtering (CF) ?
    * Making filtering decisions for an individual user based on the judgements of other users
    * Inferring individual's interest/preferences from that of other similar users
    * General idea
        * Given a user u, find similar users {u1, ..., um}
        * Predict u's preferences based on the preferences of u1, ..., um
        * User similarity can be judged based on their similarity in preferences on a common set of items.

* CF: Assumptions
    * Users with the same interest will have similar preferences
    * Users with similar preferences probably share the same interest
    * Examples
        * "interest is infomration retrieval" => "favor SIGIR papers"
        * "favor SIGIR papers" => "interest is information retrieval"
    * Sufficiently large number of user preferences are avaialable (if not, there will be a "cold start" problem)

![img]({{ '/assets/images/20181007/CS410-wk6-img-10.png' | relative_url }}){: .center-image }

### 6-8 Recommender Systems-Collaborative Filtering Part 2

![img]({{ '/assets/images/20181007/CS410-wk6-img-11.png' | relative_url }}){: .center-image }

![img]({{ '/assets/images/20181007/CS410-wk6-img-12.png' | relative_url }}){: .center-image }

* Improving User similarity meeasures
    * Dealing with missing values: set to default ratings (eg, average ratings)
    * Inverse user frequency (IUF): similar to IDF

### 6-9 Recommender Systems-Collaborative Filtering Part 3
* Summary of Recommender Systems
    * Filtering/Recommendations is "easy"
        * The user's expectation is low
        * Any recommendation is better than none
    * Filtering is "hard" 
        * Must make a binary decision, though ranking is also possible
        * Data sparseness (limited feedback information)
        * "cold start" (little information about users at the beginning)
    * Content-based vs Collaborative filtering vs Hybrid
    * Recommendation can be combined with search --> Push+Pull
    * Many advanced algorithms have been proposed to use more context information and advanced machine learning.


# CS 425 Distributed Systems

---

## Goals 

## Key Concepts

## Guiding Questions

## Readings and Resources

## Video Lecture Notes

# CS 427 Software Engineering

---

## Video Lecture Notes

### 5-1 Object-Oriented Modeling
* Abstractions: Software under development
    * Architecture 
    * OO Design
* Benefits of Modeling Notations
    * Communication
    * Documentation
    * Quality Assurance
    * Code Generation
* Unified Modeling Language (UML)
    * Class Diagram
    * Sequence Diagram


![img]({{ '/assets/images/20181007/CS427-wk6-img-1.png' | relative_url }}){: .center-image }

### 5-2 Class Diagram-Overview
* Purposes of class diagram
    * Analysis - conceptual
        * model problem, not software solution
        * can include actors outside system
    * Design - specification
        * the structure of how a software system will be written
    * Design - implementation
        * Actual classes of implementation
    * It **DOES NOT**{:.highlighted} capture how:
        * classes interact with each other
        * algorithm or behavior detail

![img]({{ '/assets/images/20181007/CS427-wk6-img-2.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-3.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-4.png' | relative_url }}){: .center-image }


### 5-3 Class Diagram-Relationships


![img]({{ '/assets/images/20181007/CS427-wk6-img-5.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-6.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-7.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-8.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-9.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-10.png' | relative_url }}){: .center-image }

### 5-4 Class Diagram-Miscellaneous

![img]({{ '/assets/images/20181007/CS427-wk6-img-11.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-12.png' | relative_url }}){: .center-image }

### 5-5 Requirements to Class Diagram
* Nouns are good candidates for classes/attributes
* Adjectives are good candidates for interfaces
* Verbs are good candidates for methods or relationships between classes

### 5-6 Sequence Diagram
* Purposes of UML Sequence Diagram
    * Used during requirements analysis
        * to refine use case descriptions
        * to find additional objects ("participating objects")
    * Used during system design
        * to refine subsystem interfaces

![img]({{ '/assets/images/20181007/CS427-wk6-img-13.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20181007/CS427-wk6-img-14.png' | relative_url }}){: .center-image }
