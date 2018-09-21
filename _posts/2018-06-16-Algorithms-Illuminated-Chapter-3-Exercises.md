---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 3 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-16
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
 - java
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

### 3.1
What is the asymptotic running time of this implementation for computing an exponential?

>Java
{:.filename}
{% highlight java linenos %}
public static int fastPower(int a, int b){
    int ans = 1;

    if ( b == 1)
        return a;
    else{
        int c = a*a;
        ans = fastPower(c,Math.floorDiv(b, 2));
    }

    if (b % 2 == 1)
        return a*ans;
    else
        return ans;
}
{% endhighlight %}

I believe it should be `log_2(b)`, answer choice **A**{: .highlighted}.

### 3.2

Give algorithm for an unimodal array, such as {2,4,6,8,7,5,3,1}:

>Java
{:.filename}
{% highlight java linenos %}
/**
 * Assumes 'in' array has length of power of 2 and is strictly 
 * unimodal with no consecutive terms that are equal.
 * 
 * @param in
 * @return 
 */
public static Comparable unimodalMax(Comparable[] in, Integer start, Integer end){
    //base case
    if (end - start == 1)
        return (in[start].compareTo(in[end]) < 0) ? in[end] : in[start];

    //divide
    Integer mid = (start+end+1)/2;

    Comparable lleft  = in[mid-1]; //last of left half
    Comparable fright = in[mid];   //first of right half

    //recurse

    //if last element in left half is less than the first element
    //of the right half, then the max element is in the right half.
    //Else, the max element is in the left half.
    return (lleft.compareTo(fright) < 0) ? unimodalMax(in, mid, end) : 
                                           unimodalMax(in, start, mid-1);
}
{% endhighlight %}

The above code will run at `O(log_2(n))`.  

### 3.3

Given a sorted array of n distinct integers, determine if there's an element that is equal to its corresponding index value in the array:

>Java
{:.filename}
{% highlight java linenos %}
/**
* Assumes 'in' array of length of power of 2 and no duplicates.
* @param in
* @return 
*/
public static boolean oneEleEqual2Index(Integer[] in, Integer start, Integer end){
    //base case
    if (start == end)
        return (in[start].compareTo(start) == 0);

    //law of syllogism
    //if last element is less than its index, then there's no way
    //that preceding elements will ever be equal to their indices.
    if (in[end].compareTo(end) < 0)
        return false;

    //divide
    if (in[end].compareTo(end) > 0){
        Integer mid = (start+end+1)/2;

        //recurse

        //check if target is in left half
        if (oneEleEqual2Index(in, start, mid-1))
            return true;

        //check if target is in right half.
        return oneEleEqual2Index(in, mid, end);
    }

    //very last element of 'in' is equal to its index.
    return true;
}
{% endhighlight %}

The above code will run at `O(log_2(n))`.  

Here's an attempt at a proof by contradiction for the logic in lines 11 to 15:

>Let **A** be a sorted array of n distinct integers with 0 based indexing.  Arbitrarily pick the kth integer and suppose its value is as large as possible while being less than its index.  Given its index is k, this means its value is k-1.  Now let's say the previous integer has a value equal to its index.  Since the previous index is k-1, then its value would be k-1.  We see then that both the kth and k-1th integers are both equal to k-1.  But **A** is a sorted array of n distinct integers.  Therefore the k-1th integer cannot be equal to its index, and in fact must be less than its index.  Since the kth integer was chosen arbitrarily, this holds for all integers from the beginning of the array up to the last integer to be less than its index and thus all integers in this region will not be equal to their corresponding index.  If the last integer in **A** is less than its index, then it can be concluded that no integer in **A** can be equal to its corresponding index.  **&#8718;**{: .highlighted}

### 3.4

Given m is a square n by n matrix with n being a power of 2 and without duplicates, develop an algorithm that only makes `O(n)` comparisons between that will return a local minimum of the matrix.  A local minimum is where all N E S W neighbors (if they exist) are larger than it.  ~~The following will find a local minimum, but not at `O(n)` comparisons unfortunately.  I believe it is `Θ(n^log_2(3) - 1)`.  While it divides the matrix into fourths, it only examines three of them.  So technically it divides the matrix into three quarters.~~
>Java
{:.filename}
{% highlight java linenos %}
/**
 * Assumes matrix m is an n by n square matrix where n is a power of 2 with
 * no duplicates.
 * 
 * Parameters (i1, j1) and (i2, j2) specify top left corner and bottom 
 * right corner of matrix m.  These are used to divide m into 4 equal
 * sub-matrices, quadrants.
 */
public static int localMin(int[][] m, int i1, int j1, int i2, int j2){
    //base case
    if (i1 == i2 && j1 == j2)
        return m[i1][j1];

    //divide and recurse
    int a = localMin(m, i1, j1, (i1+i2)/2, (j1+j2)/2);
    int b = localMin(m, i1, (j1+j2)/2+1, (i1+i2)/2, j2);

    //clever boolean logic that will identify a local minimum
    if (a < b){
        int c = localMin(m, (i1+i2)/2+1, j1, i2, (j1+j2)/2);

        if (a < c)
            return a;
        else 
            return c;
    }else{
        int d = localMin(m, (i1+i2)/2+1, (j1+j2)/2+1, i2, j2);

        if (b < d)
            return b;
        else
            return d;
    }
}
{% endhighlight %}

~~The algorithm is based off of a method I came up with to determine a local minimum in a 2 by 2 matrix with only 2 comparisons.  Here is a pictoral representation of the method:
![img]({{ '/assets/images/20180616/3-4_2by2Possibilities.svg' | relative_url }}){: .center-image }~~

~~In a 2 by 2 matrix, there's only 6 different possibilities for there to be local minima (purple) in the matrix.  There's two main groups within the 6, those that have A < B (red) and those that have A > B (Blue).  Within the red group, there's two sub groups, those with A < C (orange) and those with A > C (yellow).  Since both cases in the orange group have a local minima in position A, then that will be returned as it is guaranteed to be a local minima.  If A is not less than C, then there has to be a local minima at position C so that will be returned.  There's similar reasoning for the Blue group.  When B < D (cyan) B is returned and when B > D (green) D is returned.~~

~~Interestingly, this concept can be extended to larger matrices, so long as there's no duplicate elements and the matrix is n by n where n is a power of 2.~~

~~Edit (20180620) -- Turns out the above algorithm is WRONG.  I came up with at least two counter examples.  See, when I first made this method I tried thinking of the different ways local minima could be found in a 2 by 2 matrix.  While I got the locations correct, I failed to account for the permutations of the numbers.  Looking at the 24 different permutations, all work except for 2 of them:~~


| 2 | 3 |
| 1 | 0 |
{:.inner-borders}

| 3 | 2 |
| 0 | 1 |
{:.inner-borders}

~~This can be fixed by adding some extra booleans:~~
>Java
{:.filename}
{% highlight java linenos %}
/**
 * Assumes matrix m is an n by n square matrix where n is a power of 2 with
 * no duplicates.
 * 
 * Parameters (i1, j1) and (i2, j2) specify top left corner and bottom 
 * right corner of matrix m.  These are used to divide m into 4 equal
 * sub-matrices, quadrants.
 */
public static int localMin(int[][] m, int i1, int j1, int i2, int j2){
    //base case
    if (i1 == i2 && j1 == j2)
        return m[i1][j1];

    //divide and recurse
    int a = localMin(m, i1, j1, (i1+i2)/2, (j1+j2)/2);
    int b = localMin(m, i1, (j1+j2)/2+1, (i1+i2)/2, j2);

    //clever boolean logic that will identify a local minimum
    if (a < b){
        int c = localMin(m, (i1+i2)/2+1, j1, i2, (j1+j2)/2);

        if (a < c)
            return a;
        
        //Fix added here:
        else{
            int d = localMin(m, (i1+i2)/2+1, (j1+j2)/2+1, i2, j2);

            if (c < d)
                return c;
            else 
                return d;
        }
    }else{
        int d = localMin(m, (i1+i2)/2+1, (j1+j2)/2+1, i2, j2);

        if (b < d)
            return b;

        //Fix added here:
        else{
            int c = localMin(m, (i1+i2)/2+1, (j1+j2)/2+1, i2, j2);

            if (c < d)
                return c;
            else 
                return d;
        }
    }
}
{% endhighlight %}

Edit (20180622) -- Once again I've found a counterexample:

|15|34|12|13|
|30| 0| 6|14|
|99|98|97|96|
|95|94|93|92|
{:.inner-borders}

The updated algorithm will return 6, which is most definitely not a local minimum.  There is no saving this method, but I can remove the booleans and make it an absolute min finder:

>Java
{:.filename}
{% highlight java linenos %}
/**
 * Assumes matrix m is an n by n square matrix where n is a power of 2 with
 * no duplicates.
 * 
 * Parameters (i1, j1) and (i2, j2) specify top left corner and bottom 
 * right corner of matrix m.  These are used to divide m into 4 equal
 * sub-matrices, quadrants.
 */
public static int absoluteMin(int[][] m, int i1, int j1, int i2, int j2){
    //base case
    if (i1 == i2 && j1 == j2)
        return m[i1][j1];

    int[] quads = new int[4];

    //divide and recurse
    quads[0] = absoluteMin(m, i1, j1, (i1+i2)/2, (j1+j2)/2);
    quads[1] = absoluteMin(m, i1, (j1+j2)/2+1, (i1+i2)/2, j2);
    quads[2] = absoluteMin(m, (i1+i2)/2+1, j1, i2, (j1+j2)/2);
    quads[3] = absoluteMin(m, (i1+i2)/2+1, (j1+j2)/2+1, i2, j2);

    int min = Integer.MAX_VALUE;

    for (int i: quads)
        if (i < min)
            min = i;

    return min;
}
{% endhighlight %}


This will be `O(nlogn)`, which is better than `O(n^2)` at least.  

I've spent enough time on this and I've been unable to come up with an algorithm that meets the criteria.  Researching online, there's a couple methods that can be used, such as the [window method](http://courses.csail.mit.edu/6.006/spring11/rec/rec02.pdf) that can run in `O(n)` time.  In the interest of time, I will move on to other problems rather than implement it.

---
## Programming Challenge

### 3.5

Here is my implementation for counting inversions.  Luckily I had already implemented my own version of mergesort, so I just added a parameter to my mergesort class to keep track of inversions.  This should work for any length of list, as long as it has no duplicates.  What's nice about this is since I used a generic list of Comparable objects, it should work with any child classes.

>Java
{:.filename}
{% highlight java linenos %}
public class MergeSort {
    
    private int inversions = 0;

    public List<Comparable> mergeSort(List<Comparable> inlist){
        List<Comparable> result;
        
        //base case: list with 0 or 1 item
        if(inlist.size() <= 1)
            return inlist;
        
        //divide list into two
        List<Comparable> l1 = inlist.subList(0, inlist.size()/2);
        List<Comparable> l2 = inlist.subList(inlist.size()/2, inlist.size());
        
        //perform merge sort on both halves
        l1 = mergeSort(l1);
        l2 = mergeSort(l2);
        
        //merge the two lists
        result = merge(l1,l2);
        return result;
    }
    
    /**
     * Merges two lists in ascending order.
     * @param list1
     * @param list2
     * @return 
     */
    private List<Comparable> merge(List<Comparable> list1, List<Comparable> list2){
        List<Comparable> result = new ArrayList<>();
        
        ListIterator<Comparable> itr1 = list1.listIterator();
        ListIterator<Comparable> itr2 = list2.listIterator();
        
        while(itr1.hasNext() || itr2.hasNext()){
            while(!itr1.hasNext() && itr2.hasNext()){
                Comparable item2 = itr2.next();
                result.add(item2);
            }
            
            while(itr1.hasNext() && !itr2.hasNext()){
                Comparable item1 = itr1.next();
                result.add(item1);
            }
            
            while(itr1.hasNext() && itr2.hasNext()){
                Comparable item1 = itr1.next();
                Comparable item2 = itr2.next();
                
                if (item1.compareTo(item2) < 0){
                    result.add(item1);
                    itr2.previous();
                }else{
                    result.add(item2);
                    itr1.previous();
                    inversions+=(list1.size() - itr1.nextIndex());
                }
            }
        }
        return result;
    }

    public int getInversions() {
        return inversions;
    }
    
    public static void printList(List<Comparable> inlist){
        String result = "";
        
        for (Object o : inlist){
            result += o.toString() + " ";
        }
        
        System.out.println(result);
    }
}
{% endhighlight %}
