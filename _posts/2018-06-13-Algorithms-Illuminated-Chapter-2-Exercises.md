---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 2 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-13
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

### 2.1

Here is my proof for this problem:

 ![img]({{ '/assets/images/20180613/Algorithms-Illuminated-pt1-ch2-ex1.svg' | relative_url }}){: .latexequation }

and here are my reasonings for each numbered step:

1. Constant multiple property of logs.
2. `f = O(g)` was given.
3.  Function composition of Big-O
    * `log(n) = O(log(n)` by identity property of Big-O.
    * `f = O(g)` was given.
4. Multiplicative property of Big-O.
5. Constant multiple property of Big-O.

Thus, **A**{: .highlighted} should be the right answer.

### 2.2

~~Because of the composition property of Big-O, the proposed statement is true.  Answer choice **A**{: .highlighted} is definitely correct, and I believe **D**{: .highlighted} is just another way of stating what `f = O(g)` means mathematically.  So both **A and D**{: .highlighted} are correct.~~

Turns out that my previous answer was incorrect.  I was presented a counter example, `f = 2n` and `g = n`.  Thus, A is not correct, and it should be **C and D**{: .highlighted}.

### 2.3

![img]({{ '/assets/images/20180613/2.3.jpg' | relative_url }}){: .center-image}

**A, C, E, D, B**{: .highlighted}

### 2.4

![img]({{ '/assets/images/20180613/2.4.jpg' | relative_url }}){: .center-image}

**E, A, B, D, C**{: .highlighted}

### 2.5

![img]({{ '/assets/images/20180613/2.5.jpg' | relative_url }}){: .center-image}

**A, E, C, B, D**{: .highlighted}