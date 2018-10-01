---
layout: post
title: "MCS 1st Semester Week 5 Notes"
description: My notes for this week's lectures.
date: 2018-09-30
comments: true
tags:
 - UIUC-MCS
---

# CS 410 : Text Information Systems
---
## Goals and Objectives
* Explain the similarity and differences in the three different kinds of feedback, i.e., relevance feedback, pseudo-relevance feedback, and implicit feedback.
* Explain how the Rocchio feedback algorithm works.
* Explain how the Kullback-Leibler (KL) divergence retrieval function generalizes the query likelihood retrieval function.
* Explain the basic idea of using a mixture model for feedback.
* Explain some of the main general challenges in creating a web search engine.
* Explain what a web crawler is and what factors have to be considered when designing a web crawler.
* Explain the basic idea of Google File System (GFS).
* Explain the basic idea of MapReduce and how we can use it to build an inverted index in parallel.
* Explain how links on the web can be leveraged to improve search results.
* Explain how PageRank algorithm works.

## Guiding Questions
* What is relevance feedback? What is pseudo-relevance feedback? What is implicit feedback?
* How does Rocchio work? Why do we need to ensure that the original query terms have sufficiently large weights in feedback?
* What is the KL-divergence retrieval function? How is it related to the query likelihood retrieval function?
* What is the basic idea of the two-component mixture model for feedback?
* What are some of the general challenges in building a web search engine?
* What is a crawler? How can we implement a simple crawler?
* What is focused crawling? What is incremental crawling?
* What kind of pages should have a higher priority for recrawling in incremental crawling?
* What can we do if the inverted index doesn’t fit in any single machine?
* What’s the basic idea of the Google File System (GFS)?
* How does MapReduce work? What are the two key functions that a programmer needs to implement when programming with a MapReduce framework?
* How can we use MapReduce to build an inverted index in parallel?
* What is anchor text? Why is it useful for improving search accuracy?
* What is a hub page? What is an authority page?
* What kind of web pages tend to receive high scores from PageRank?
* How can we interpret PageRank from the perspective of a random surfer “walking” on the Web?
* How exactly do you compute PageRank scores?
* How does the HITS algorithm work?

## Additional Readings and Resources
* C. Zhai and S. Massung. Text Data Management and Analysis: A Practical Introduction to Information Retrieval and Text Mining, ACM Book Series, Morgan & Claypool Publishers, 2016. Chapters 7 & 10

## Key Phrases and Concepts
* Relevance feedback
* Pseudo-relevance feedback
* Implicit feedback
* Rocchio feedback
* Kullback-Leiber divergence (KL-divergence) retrieval function
* Mixture language model
* Scalability and efficiency
* Spams
* Crawler, focused crawling, and incremental crawling
* Google File System (GFS)
* MapReduce
* Link analysis and anchor text
* PageRank

## Video Lecture Notes
### 5.1 : Feedback in Text Retrieval
![img]({{ '/assets/images/20180930/CS410-wk5-img-1.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-2.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-3.png' | relative_url }}){: .center-image }

### 5.2 : Feedback in Vector Space Model - Rocchio
* Feedback in vector space model
    * how can a TR system learn from examples to improve retrieval accuracy?
        * positive examples: docs known to be relevant
        * negatvie examples: docs known to be non-relevant
    * general method: query modicfication
        * adding new (weighted) terms (query expansion)
        * adjusting weights of old terms

* Rocchio in Practice
    * Negative (non-relevant) examples are not very important
    * Often truncate the vector (ie, consider only a small number of words that have highest weights in the centroid vector)(efficiency concern)
    * Avoid "over-fitting" (keep relatively high weight on the original query weights)
    * Can be used for relevence feedback and pseudo feedback (&Beta; should be set to a larger value for relevance feedback than for pseudo feedback)
    * Usually robust and effective


![img]({{ '/assets/images/20180930/CS410-wk5-img-4.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-5.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-6.png' | relative_url }}){: .center-image }

### 5.3 : Feedback in Text Retrieval - feedback in LM
* Feedback with LM
    * Query likelihood method can't naturally support relevance feedback
    * Solution:
        * Kullback-Leibler( KL) divergence retrieval model as a generalization of query likelihood
        * Feedback is achieved through query model estimation/updating


![img]({{ '/assets/images/20180930/CS410-wk5-img-7.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-8.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-9.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-10.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-11.png' | relative_url }}){: .center-image }

* Summary of feedback in Text Retrieval
    * Feedback = learn from examples
    * Three major feedback scenarios
        * Relevance, pseudo, and implicit feedback
    * Rocchio for VSM (vector space model)
    * Query Model estimation for LM
        * Mixture model
        * many other methods (eg, relevance model) have been proposed


### 5.4 : Web Search: Introduction & Web Crawler
* Web Search: Challenges & Opportunities
    * Challenges
        * scalability
            * how to handle the size of the web and ensure completeness of coverage?
            * how to serve many user queries quickly?
        * low quality information and spams
        * dynamics of the web
            * new pages are constantly created and some pages may be updated very quickly
    * Opportunities
        * many additional heuristics (eg links) can be leveraged to improve search accuracy

*  Component I: Crawler/Spider/Robot
    * Building a "toy crawler" is easy
        * Start with a set of "seed pages" in a priority queue
        * fetch pages from the web
        * parse fetched pages for hyperlinks; add them to the queue
        * follow the hyperlinks in the queue
    * A real crawler is much more complicated...
        * Robustness (server failure, trap, etc)
        * Crawling courtesy (server load balance, robot exlclusion, etc)
        * Handling file types (images, PDF files, etc)
        * URL extensions (cgi script, internal references, etc)
        * recognize redundant pages (identical and duplicates)
        * discover "hidden" urls (eg, truncating a long url)
* Major Crawling strategies
    * Breadth-first is common (balance server load)
    * parallel crawling is natural
    * variation: focused crawling
        * targeting at a subset of pages (eg, all pages about "automobiles")
        * typically given a query
    * How to find new pages (they may not linked to an old page!)
    * Incremental/repeated crawling
        * need to minimize reource overhead
        * can learn form the past experience (updated daily vs monthly)
        * target at : 1) frequently updated pages; 2) frequently accessed pages

![img]({{ '/assets/images/20180930/CS410-wk5-img-12.png' | relative_url }}){: .center-image }

### 5.5 : Web indexing
* Overview of web indexing 
    * standard IR techniques are the baseis, but insufficient
        * scalability
        * efficiency
    * Google's gontributions:
        * google file system (gfs): distributed file system
        * mapreduce: software framework for parallel computation
        * hadoop: open source implementation of mapreduce

* Mapreduce: a framework for parallel programming
    * minimize effort of programmer for simple parallel processing tasks
    * Features
        * hide many low-level details (network, storage)
        * build-in fault tolerance
        * automatic load balancing


![img]({{ '/assets/images/20180930/CS410-wk5-img-13.png' | relative_url }}){: .center-image }
![img]({{ '/assets/images/20180930/CS410-wk5-img-14.png' | relative_url }}){: .center-image }

### 5.6 : Link Analysis 
* Ranking Algorithms for Web Search
    * Standard IR (information retrieval) models apply but aren't sufficient
        * Different information needs
        * documents have additional information
        * information quality varies a lot
    * Major extensions
        * exploiting links to improve scoring
        * exploiting clickthroughs for massive implicit feedback
        * in general, rely on machine learning to combine all kinds of features

* Pagerank: capturing page "popularity"
    * intutions
        * links are like citations in literature
        * a page taht is cited often can be expected to be more useful in general
    * pagerank is essentially "citation counting", but improves over simple counting
        * consider "indirect citations" (being cited by a highly cited paper counts a lot...)
        * smoothing of citations (every page is assumed to have a non-zero pseudo citation count)
    * PageRank can also be interpreted as random surfing (thus capturing popularity)

### 5.7 : Link Analysis Part 2
* The pagerank algorithm
    * Random surfing model: At any range,
        * with probability &alpha; randomly jumping to another page
        * with probability (1-&alpha;), randomly picking a link to follow.
            * p(di): PageRank score of di = average probability of visiting page di
* PageRank in Practice
    * computation can be quite efficient since M is usually sparse
    * Normalization doesn't affect ranking, leading to some variants of the formula
    * The zero-outlink problem: p(di)'s don't sum to 1 
        * one possible solution = page-specific damping factor (&alpha; =1.0 for a page with no outlink)
    * Many extensions (eg, topic-specific PageRank)
    * Many other applications (eg, social network analysis)

![img]({{ '/assets/images/20180930/CS410-wk5-img-15.png' | relative_url }}){: .center-image }
### 5.8 : Link analysis - Part 3 (optional)
* HITS: Capturing Authorities & Hubs
    * Intuitions
        * Pages that are widely cited are good authorities
        * Pages taht cite many other pages are good hubs
    * The key idea of HITS (Hypertext-Induced Topic Search)
        * Good authorities are cited by good hubs
        * Good hubs point to good authorities
        * Iterative reinforcement...
    * Many applications in graph/network analysis

* Summary
    * link information is very useful
        * anchor text
        * pagerank
        * HITS
    * Both pagerank and HITS have many applications in analyzing other graphs or networks


![img]({{ '/assets/images/20180930/CS410-wk5-img-16.png' | relative_url }}){: .center-image }


# CS 425 : Distributed Systems
---

## Goals 

## Key Concepts

## Guiding Questions

## Readings and Resources

## Video Lecture Notes

# CS 427 : Software Engineering
---

## Goals and Objectives

## Video Lecture Notes

### 4.1 : Definition of Software Architecture
* Definitions of Software ARchitecture
    * "A software system's architecture is the set of of **princiapal design decisions**{:.highlighted} about the system."
    * The software architecture of a program or computing system is the structure or strucutures of the system, whcih comprise **software elements**{:.highlighted}, the **externally visible properties**{:.highlighted} of those elements, and the **relationshipts**{:.highlighted} among them."
    * A model of software architecture "consists of three components: **elements, form, and rationale**{:.highlighted}."
        * Elements (what): Processing, data, or connecting elements
        * Form (how): Constraints (properties, relationships) on the elements
        * Rationale (why): System constraints, often derived from system requirements

* Benefits of sofware architecture
    * Understanding 
    * Reuse
    * Construction
    * Evolution
    * Analysis
    * Management

### 4.2 : Software Architect
* Important skills of software architects
    * "software architects should **design, develop, nurture, and maintain**{:.highlighted} the architecture of the software-intensive systems they are involved with."
        * Domain Knowledge
        * Software Development Expertise
        * Communication Skills
        

![img]({{ '/assets/images/20180930/CS427-wk5-img-1.png' | relative_url }}){: .center-image }

### 4.3 : Abstraction
* Definition of Abstraction
    * Removing detail to **simplify and foucus attention**{:.highlighted}
    * Generalizing to **identify the common core or essence**{:.highlighted}
* Importance of Abstraction in CS
    * "Once you realize that computing is all about **constructing, manipulating, and reasoning about abstractions**{:.highlighted}, it becomes clear that an important prerequisiite for writing(good) computer programs is the ability to **handle abstractions in a precise manner**{:.highlighted}."
    * "Computer science is not computer programming. Thinking like a computer scientist means more than being able to program a computer. It requires **thinking at multiple levels of abstraction**{:.highlighted}."

* Types of Abstraction in Software Engineering
    * **Procedural**{:.highlighted} abstraction
        * Naming a sequence of instructions
        * Parameterizing a procedure
    * **Data**{:.highlighted} abstraction
        * Naming a collection of data
        * Data type defined by a set of procedures
    * **Control**{:.highlighted} abstraction
        * Without specifying all register/binary-level steps
    * **Performance**{:.highlighted} abstraction O(N)

* Software Development process also has abstractions
    * Requirements (What)
    * Architecture
    * OO Design
    * Implementation (How)

* How to get good abstractions
    * learn from others
    * generalize from examples
    * look for/eliminate duplication

* Abstractions can fool you
    * suppose a collection c has operation: getItemNumbered(int index)
    * How do you iterate?
    
>C++
{:.filename}
{% highlight C++ linenos %}
for (i = 0; i < c.length(); i++){
    c.getItemNumbered(i); 
}
{% endhighlight %}


* But what if the collection is a linked list?

### 4.4 : Modularity
* Functional Independence
    * Cohesion (Good)
        * Measure of interconnection within a module
        * The degree to which one part of a module depends on another
        * Maximize cohesion
    * Coupling (Less desirable)
        * Measure of interconnection among modules
        * The degree to which one module depends on others
        * Minimize coupling

* Major types of cohesion
    * Coincidental - grouped by chance  (Low cohesion)
    * logical - same idea
    * temporal - same time
    * procedural - one after another
    * communicational - shared data
    * sequential - output of one being input of the other
    * functional - a single well-defined task (high cohesion)

* Information hiding
    * EAch module should hide a design decision from others
    * Ideally, one design decision per module, but usually design decisions are closely related

* Example design decisions
    * Representation of data
    * Use of a particular software package
    * Use of a particular printer
    * Use of a particular operating system
    * Use of a particular algorithm

* Other reasons for modularity
    * collaborative/distributed development: reduced communication
    * security - compartmentalization
    * reliability - localization of failure
    * Conway's law: the architecture of a system is the same as the structure of the group that developed it.

* ways to achieve modularity
    * reuse a design with good modularity
    * think about and hide design decisions
    * reduce coupling and increase cohesion
    * eliminate duplication
    * reduce impact of changes

### 4.5 : Architecture
* Three views of software architecture
    * Static view
    * Dynamic view
    * Deployment view
* Components and connectors

### 4.6 : Architectural Styles
* Architectural Style
    * **Vocabularly**{:.highlighted} of components and connectors
    * Compositions **constraints**{:.highlighted}
* Client-server style
    * Example: SVN
    * Easy to add new clients to structure
* Layered/Tiered style: 3-tier/n-tier client-server (eg local file system)
    * Presentation tier
    * Logic tier
    * data tier
* Pipe-and-filter style
    * eg cat contatctbook \| grep "illinois" \| sort > illini

### 4.7 : MVC Pattern
* Architectural Pattern
    * A **general, reusable**{:.highlighted} solution to a **commonly occuring**{:.highlighted} problem in software architecture within a given context.
    * Not the same as architectural style
        * **Vocabularly**{:.highlighted} of components and connectors
        * Composition **constraints**{:.highlighted}

* Model-View-Controller Pattern


![img]({{ '/assets/images/20180930/CS427-wk5-img-2.png' | relative_url }}){: .center-image }
