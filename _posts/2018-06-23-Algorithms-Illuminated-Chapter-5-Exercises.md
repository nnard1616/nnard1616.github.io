---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 5 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-23
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
 - java
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}


### 5.1

Given the following array, that has gone through one round of partitioning during a quicksort procedure, which of the elements could be a pivot?

| 3 | 1 | 2 | 4 | 5 | 8 | 7 | 6 | 9 |
{:.inner-borders}

4, 5, or 9 could be the pivot.

### 5.2

Suppose array `A` has a length of l.  If some &alpha; is strictly between 0 and 1/2, then the probability for a pivot, p, being chosen such that both subproblems are at least &alpha;*l can be proven by the following:

>Consider three indices, p, p-&alpha;\*l, and p+&alpha;\*l where p is the index of the pivot.  In order for the subproblems to have a length at least &alpha;\*l then p-&alpha;\*l must be >= 0 and p+&alpha;\*l must be <= l.  Hence p >= &alpha;\*l and p <= l-&alpha;\*l.  This implies the size of the range of acceptable pivot selections is (l - &alpha;\*l) - &alpha;\*l = l - 2&alpha;\*l = l(1 - 2&alpha;).  The factor (1 - 2&alpha;) represents the fraction of the length of array `A` in which the pivot can be placed and satisfy the condition.  Therefore, the probability of randomly selecting a pivot that satisfies the above condition is (1 - 2&alpha;).**&#8718;**{: .highlighted}

Thus, **C**{: .highlighted} is the correct answer.

### 5.3

If every partition were to meet the conditions mentioned in the previous problem, then what would be the minimal and maximal successive recursive calls made before reaching the base case in either of the two upper level recursive calls?

To answer this, first consider if the pivot were chosen to perfectly divide the array into two equal parts at each recursion level.  For this procedure, it was proven that the number of recursive calls is approximately log_2(n).  The base 2 is because each part is 1/2 of the original n elements.

For the conditions mentioned in the previous problem, assume the pivot is chosen so that the left subarray just meets the length requirement.  This would split the original array into an **&alpha;n**{:.highlighted} part and an **&alpha;n + n(1 - 2&alpha;) = (1 - &alpha;)n**{:.highlighted} part.  Hence the two parts are **&alpha;**{:.highlighted} and **(1 - &alpha;)**{:.highlighted} fractions of the original n elements.  In fact, they also represent the best and worst way to divide the original elements.  So following the example of the log_2(n), then the best and worst case number of recursive calls will be <img src="http://latex.codecogs.com/svg.latex?\color{Orange}log_{\alpha^-1}(n)" class="latexinline"/> and <img src="http://latex.codecogs.com/svg.latex?\color{Orange}log_{(1-\alpha)^-1}(n)" class="latexinline"/>, respectively. Note that the bases both need to be risen to the -1 power as n is multiplied by them and not divided (similar to how 1/2^-1 = 2 for halving).  Answer choice **B**{:.highlighted} matches these values.

### 5.4

For an array with sufficiently large number of elements, n, the best and worst case number of recursive calls made before hitting a base case will be **&Theta;(logn)**{:.highlighted} (if pivot is median every time) and **&Theta;(n)**{:.highlighted} (if pivot is smallest or largest value every time), respectively.  Therefore **B**{:.highlighted} is the correct answer.


### *5.5

In section 5.6 it was proven that all comparison-based sorting algorithms have a lower bound of **&Omega;(nlogn)**{:.highlighted} comparisons.  We now want to extend this to apply to the expected running time of randomized comparison-based sorting algorithms.  

I am unsure how to proceed for this proof, I would imagine I must prove that the expected number of comparisons made using randomized comparison-based sorting is at least `nlogn`{:.highlighted}.  Unfortunately I am unsure how to prove that though and I need to move on to other things.  Maybe I'll return to this another day.

---
## Programming Challenge

### 5.6

Implement quicksort:

>Java
{:.filename}
{% highlight java linenos %}
public class QuickSort {
    private static Random rand = new Random();
    
    public static void quickSort(int[] a, int l, int r){
        //base case
        if (l >= r)
            return;
        
        int i = choosePivot(l, r);
        
        swap(a, i, l);
        
        //partition returns index of new pivot, j
        int j = partition(a, l, r);
        
        
        quickSort(a, l, j-1);
        quickSort(a, j+1, r);
        
    }
    
    private static int choosePivot(int l, int r){
        return rand.nextInt(r-l+1)+l;
    }
    
    private static int partition(int[] a, int l, int r){
        int p = a[l];
        int i = l+1;
        
        for (int j = l + 1; j <= r; j++)
            if (a[j] < p)
                swap(a, i++, j);
        
        swap(a, l, i-1);
        
        return i-1;
    }
    
    private static void swap(int[] a, int i, int j){
        int temp = a[j];
        a[j] = a[i];
        a[i] = temp;
    }
}
{% endhighlight %}
