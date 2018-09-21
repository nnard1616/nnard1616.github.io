---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 4, Week 4"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-30
comments: true
tags:
 - coursera
 - roughgarden
 - algorithms
 - c++
---


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

## Quiz

### P1
![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex1.png' | relative_url }}){: .center-image }

The first one is most definitely false as the generic local search algorithm is only guaranteed to provide a local optimal solution, not a general optimal solution.


### P2
![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex2.png' | relative_url }}){: .center-image }

It will certainly finish in polynomial number of iterations, that's the point of the approximate heuristic algorithm.

### P3
![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex3.png' | relative_url }}){: .center-image }
This problem is taken directly from Kleinberg/Tardos' Algorithm Design, Theorem 12.5.  Please feel free to refer to that text for more in depth analysis.

The proof for that theorem is fairly involved for a binary partition, and even more complicated for k partitions.  Refer to my simplified proof below:

![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex3-1.svg' | relative_url }}){: .latexequation }

Note that C is the sum of the weights of the set of edges that cross the clusters, C' is that of the optimal solution's.  W is the sum of all of the weights of the edges and A<sub>i</sub> is the sum of the weights of edges exclusively in that corresponding cluster.  

See the text for background on why the first line is true.

### P4
![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex4.png' | relative_url }}){: .center-image }

The probability approaches 100%. (say a sample space of 2 with probability of 99% and -100 with probability of 1%)

### P5
![img]({{ '/assets/images/20180830/Algorithms-pt4-wk4-ex5.png' | relative_url }}){: .center-image }

In the smallest case, which represents the case with the maximum probability, consider a sample space of 0 and 2 with equal probability.  This will be as large as the probability can go, due to there not being any negative numbers. So 50% is the largest it can be.

---
## Programming Assignment

This assignment we are tasked with determining which of six different 2SAT problems are satisfiable.  Since all of the clauses depend on just two variables, it was easy to make a graph representation (via the corresponding equivalent implications implied by the clauses).  With this representation, I was able to re-use my [SCC solution]({% post_url 2018-07-09-Coursera-Roughgarden-Algorithms-Pt-2-Wk-1 %}) to look for SCC's.  Those that are not satisfiable will have a paradox, ie ~t -> t, within a SCC.  One could search through the SCCs individually to look for paradoxes, but I found an interesting property that permitted me to quickly determine if a paradox exists based on the number and sizes of the SCCs.  If there's an even number of SCCs with sizes larger than 1, then no paradox exists.  If there's an odd number of SCCs with size larger than 1, then a paradox exists.  So as a couple of examples:  

* 5 3 3 1 1 1 1.... would correspond to paradox present as there's an odd number of SCCs with size 5.
* 8 8 5 5 1 1 1.... would correspond to no paradox present as there's even numbers of SCCs with size 8 & 5.

So I just calculated and printed the SCCs sorted by decreasing size for each 2SAT problem and determined satisfiability based on the number of SCC sizes.

Since I didn't change much in my SCC code nor did I write any new interesting code, I refrain from posting any code.





