---
layout: post
title: "MCS 1st Semester Week 8 Notes"
description: My notes for this week's lectures.
date: 2018-10-17
comments: true
tags:
 - UIUC-MCS
---
# CS 410 Text Information Systems

---

(under constructior construction)

## Goals and Objectives

## Guiding Questions

## Additional Readings and Resources

## Key Phrases and Concepts

## Video Lecture Notes


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


