---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 4 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-21
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}


### 4.1

![img]({{ '/assets/images/20180621/Selection_041.png' | relative_url }}){: .center-image }

I believe `b^d` can be interpreted as the rate at which the work-per-subproblem is shrinking, so **D**{: .highlighted} should be the correct answer.

### 4.2

With a recurrence of `T(n) <= 7T(n/3) + O(n^2)`, the running time of the algorithm would be **B**{: .highlighted}, `O(n^2)`.

### 4.3

With a recurrence of `T(n) <= 9T(n/3) + O(n^2)`, the running time of the algorithm would be **C**{: .highlighted}, `O(n^2log(n))`.

### 4.4

With a recurrence of `T(n) <= 5T(n/3) + O(n)`, the running time of the algorithm would be **C**{: .highlighted}, `O(n^log_3(5))`.

### 4.5

With a recurrence of `T(n) <= T(sqrt(n)) + 1`, I believe the running time of the algorithm would be **B**{: .highlighted}, `O(log(log(n)))`.  Think of sqrt(n) = sqrt(2^log(n)) = (2^log(n))^(1/2) = 2^(log(n)/2).
