---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 3, Week 4"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-20
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
![img]({{ '/assets/images/20180820/Algorithms-pt3-wk4-ex1.png' | relative_url }}){: .center-image }

For algorithm 1, there's a bias towards set 1 that potentially prevents seeing an optimal solution in set 2.  To make algorithm 1 to work, you would need to repeat the algorithm but in reverse (set 2 then set 1), and take the best solution of the two.

For algorithm 2, imagine that the two sets are filled separately.  Say there's a tiny bit of room left in both sets.  If you were to combine these two sets, these two pieces of space might sum to a large enough space to fit an extra item.  As such, if you follow algorithm 2, items may be included that won't fit into either separated set after splitting them up.  Therefore algorithm 2 can't be guaranteed to produce a solution.

### P2
![img]({{ '/assets/images/20180820/Algorithms-pt3-wk4-ex2.png' | relative_url }}){: .center-image }
Assuming 2D array dimensions are adjusted to prevent seg faults, the order of the nested for loops shouldn't matter.

### P3
![img]({{ '/assets/images/20180820/Algorithms-pt3-wk4-ex3.png' | relative_url }}){: .center-image }
The optimal BST is below:
![img]({{ '/assets/images/20180820/ex3-1.svg' | relative_url }}){: .latexequation }
Then the minimum-possible average is computed using the expected value formula.

### P4
![img]({{ '/assets/images/20180820/Algorithms-pt3-wk4-ex4.png' | relative_url }}){: .center-image }

All of these can be computed in **O(mn)**{:.highlighted} time.  In fact, the permutation one can be done in **O(n)**{:.highlighted} without dynamic programming.
 
### P5
![img]({{ '/assets/images/20180820/Algorithms-pt3-wk4-ex5.png' | relative_url }}){: .center-image }
For MWIS, only the two previous solutions need to be remembered, so it is **O(1)**{:.highlighted}.
For sequence alignment, only two values of i subproblems's solutions need to be remembered, but for all values of j.  So **O(n)**{:.highlighted}.
For optimal binary search trees, previous subproblems remain relevant throughout the entire computation, so it is **O(n<sup>2</sup>)**{:.highlighted}.

---
## Programming Assignment

### P1

The first problem had us solve a 100 item knapsack problem.  Implementing the algorithm from lecture, which uses a double for loop, made quick work of it.  Since the code was minimal and uninteresting, I will refrain from posting it.

### P2 

This follow up problem is a little more interesting.  With 2000 items for our knapsack, we have a much larger problem that requires a more clever implementation.  For this, I used a recursive function backed by a hash table permitting memoization. Below is the recursive function I came up with to solve this problem:

>C++
{:.filename}
{% highlight C++ linenos %}
//tab is our memoization table, items contains a mapping from item i to its 
//corresponding (vi, wi) pair.
int rec (int i, int x, map<pair<int,int>,int>& tab, map<int, pair<int, int>>& items){

    if (i == 0)
        return 0;

    try{
        return tab.at(pair<int, int>(i,x));
    } catch (std::out_of_range){

        // check if x - wi is non negative
        if (x - items[i].second >= 0)

            // return max of A[i-1, x] and A[i-1, x-wi] + vi
            tab[pair<int, int>(i,x)] = max(rec(i-1, x, tab, items), 
                                           rec(i-1, x - items[i].second, 
                                                                    tab, items) 
                                           + items[i].first);
        else
            // return A[i-x, x] since x-wi is a negative number.
            tab[pair<int, int>(i,x)] = rec(i-1, x, tab, items);

        return tab[pair<int, int>(i,x)];
    }
}
{% endhighlight %}

Runs in about 45 seconds.