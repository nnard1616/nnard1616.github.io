---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 6 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-25
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
 - java
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

### 6.1

In order for the first recursive call to be passed a subarray of length at most **&alpha;n**{:.highlighted}, where **&alpha;**{:.highlighted} is strictly between 0.5 and 1, the pivot must be located between two indices at **&alpha;n**{:.highlighted} and **n - &alpha;n**{:.highlighted} from one endpoint of the array.  The size of this region is **&alpha;n - (n - &alpha;n) = n(2&alpha; - 1)**{:.highlighted}.  Thus **2&alpha; - 1**{:.highlighted} is the fraction of the array that the pivot can be in and is therefore the probability of the conditions being satisfied.  

**D**{:.highlighted} is the correct answer.

### 6.2

This will require the same reasoning used in problem [5.3]({% post_url 2018-06-23-Algorithms-Illuminated-Chapter-5-Exercises %}).  The maximum number of successive recursive calls occurs in the worst case scenario, which is when the subarray is **&alpha;n**{:.highlighted}.  Hence, the answer will be <img src="http://latex.codecogs.com/svg.latex?\color{Orange}log_{\alpha^-1}(n)" class="latexinline"/>.  

Choice **A**{:.highlighted} is equivalent to this.  

### 6.3

A linear deterministic algorithm for finding a "weighted median" of an array of n distinct elements each with a corresponding weight, w, could be the following implementation:

>Java
{:.filename}
{% highlight java linenos %}
public static Doublet weightedMedian(Doublet[] a, int l, int r){
    //base case
    if (l == r)
        return (Doublet)dselect(a, l);

    //ith statistic, choose to be middle ith.
    int i = (l+r)/2;

    //a will be rearranged so that d is in its proper ith position and every
    //element to the left will be less than, and every element to the right
    //will be greater than d.
    Doublet d = (Doublet)dselect(a, i);

    int fullSum  = weightedSum(a, 0, a.length-1);
    int rightSum = weightedSum(a, i+1, a.length-1);
    int leftSum  = weightedSum(a, 0, i-1);

    //recurse on left half
    if (leftSum > fullSum/2)
        return weightedMedian(a, l, i-1);

    //recurse on right half
    if (rightSum > fullSum/2)
        return weightedMedian(a, i+1, r);

    //we got lucky, d is a weighted median
    return d;
}

/**
 * Sum of weights from l to r inclusive.
 * @param a
 * @param l
 * @param r
 * @return 
 */
private static int weightedSum(Doublet[] a, int l, int r){
    int result = 0;
    for (int i = l; i <= r; i++)
        result += a[i].W;
    return result;
}

/**************************************************************************/
/*  Doublet Implementation                                                */
/**************************************************************************/
public class Doublet implements Comparable{
    public final int X;
    public final int W;

    public Doublet(){
        this(0, 0);
    }

    public Doublet(int x, int w){
        X = x;
        W = w;
    }

    @Override
    public int compareTo(Object o) {
        if (o.getClass() != this.getClass())
            throw new IllegalArgumentException("doublet compared with "
                                             + "non-doublet");

        Doublet d = (Doublet)o;

        if (this.X < d.X)
            return -1;
        if (this.X > d.X)
            return 1;
        return 0;
    }

    @Override
    public boolean equals(Object o) {
        if (o.getClass() != this.getClass())
            throw new IllegalArgumentException("doublet compared with "
                                             + "non-doublet");
        Doublet d = (Doublet)o;

        if (this.X == d.X)
            return true;
        return false;
    }

    @Override
    public String toString() {
        return "X: " + X + " W: " + W;
    }
}
{% endhighlight %}

It first finds the middle ith statistic in the array via dselect.  Upon returning the ith statistic in the array, dselect will partition the array around the ith statistic as a side effect.  The algorithm can then compute the left and right sums, and whichever half has a larger sum must contain the weighted median.  So then it will recurse on that half.  This repeats until the weighted median is found.  At each recursion level one recursive call is made and each time it is called on an array that is half the size of the previous array.  This, along with the fact that every operation is linear, implies the algorithm is indeed deterministic and linear.

However, there is a possibility for there to be at most 2 weighted medians.  But this is easy to account for as the weighted medians will be neighboring statistics.  So just check if i-1th and i+1th statistics are weighted medians as well after running the above algorithm.  Since this is always a constant 2 extra calculations, this does not change the linearity of the algorithm.

### 6.4

If one were to modify the DSelect algorithm to use groups of 7 instead of 5, it will still run in **O(n)**{:.highlighted}.  The proof is the same as in the text, only the recurrence relation will be: 

<img src="http://latex.codecogs.com/svg.latex?\color{Orange}T(n) \leq l\cdot \frac{1}{7}\cdot n + l \cdot \frac{7}{10}\cdot n + cn" class="latexinline"/>

The strategy used in the text was to come up with a value for c so that this relation simplifies to <img src="http://latex.codecogs.com/svg.latex?\color{Orange}T(n) \leq l \cdot n" class="latexinline"/>. One can use c = 11*l/70 so it's clear that using groups of 7 will still be **O(n)**{:.highlighted}.

Groups of 3, however, the algorithm, in its current design, will no longer be linear.  Trying to do the same as above for groups of 7, it is determined that c = -l/30.  But both c and l must be positive values so it is impossible to be **O(n)**{:.highlighted}.

However, recent literature proposes ways to make it **O(n)**{:.highlighted}.  See [here](https://arxiv.org/abs/1409.3600).

---
## Programming Challenge

### 6.5

We're asked to implement rselect.  Both rselect and dselect are implemented below:

>Java
{:.filename}
{% highlight java linenos %}
public class Selection {
    private static final int GROUP = 5;
    private static Random rand = new Random();
    
    /**
     *  Assumes ith statistic exists.
     * 1st --> i = 0
     * 2nd --> i = 1, etc.
     * 
     * @param a
     * @param i
     * @return value at ith statistic
     */
    public static Comparable dselect(Comparable[] a, int i){
        //base case
        if (a.length == 1)
            return a[0];
        
        //c will contain medians of the groups of GROUP
        Comparable[] c = new Comparable[a.length/GROUP];
        
        for (int h = 0; h < a.length/GROUP; h++)
            c[h] = median(Arrays.copyOfRange(a, h*GROUP, (h+1)*GROUP));
        
        //p will be the median of the medians in c.
        Comparable p = -1;
        
        
        //there's potential for c to be smaller than GROUP
        
        //c's length is less than the GROUP divisor
        if (c.length == 0)
            p = median(a);
        else //c's length is longer than GROUP divisor
            p = dselect(c, (c.length-1)/2);
        
        //pivot index
        int indexOfp = getIndexOf(a, p);
        
        //switches first element with chosen pivot
        swap(a, indexOfp, 0);
        
        //partition returns index of new pivot, j
        int j = partition(a, 0, a.length-1);
        
        if (j == i)
            return p;
        else if ( j > i)
            return dselect(Arrays.copyOfRange(a, 0, j), i);
        else
            return dselect(Arrays.copyOfRange(a, j+1, a.length), i-j-1);
    }
    
    public static Comparable rselect(Comparable[] a, int i){
        //base case
        if (a.length == 1)
            return a[0];
        
        //randomly pick pivot
        int p = rand.nextInt(a.length);
        
        //switches first element with chosen pivot
        swap(a, p, 0);
        
        //partition returns index of new pivot, j
        int j = partition(a, 0, a.length-1);
        
        if (j == i)
            return a[p];
        else if ( j > i)
            return rselect(Arrays.copyOfRange(a, 0, j), i);
        else
            return rselect(Arrays.copyOfRange(a, j+1, a.length), i-j-1);
    }
    
    /**
     * Returns index of first occurrence of value.  -1 if value is not in a.
     * @param a
     * @param value
     * @return 
     */
    private static int getIndexOf(Comparable[] a, Comparable value){
        for (int i = 0; i < a.length; i++)
            if (a[i].equals(value))
                return i;
        
        return -1;
    }
    
    private static Comparable median(Comparable[] a){
        Arrays.sort(a); 
        
        return a[(a.length-1)/2];
    }
}
{% endhighlight %}
