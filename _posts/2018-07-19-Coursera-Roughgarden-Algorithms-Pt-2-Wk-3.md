---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 2, Week 3"
description: My thoughts and solutions for this week's exercises.
date: 2018-07-19
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
>Suppose you implement the functionality of a priority queue using a sorted array (e.g., from biggest to smallest). What is the worst-case running time of Insert and Extract-Min, respectively? (Assume that you have a large enough array to accommodate the Insertions that you face.)

Assuming the priority queue's push operation uses a linear search to find where to put new items, then it will be **&Theta;(n)**{:.highlighted} time for insertion.  However, this can be improved to **&Theta;(logn)**{:.highlighted} by using binary search.  Since the array is sorted, it takes constant time to retrieve the min (or max) of the array.

### P2
>Suppose you implement the functionality of a priority queue using an unsorted array. What is the worst-case running time of Insert and Extract-Min, respectively? (Assume that you have a large enough array to accommodate the Insertions that you face.)

Assuming the priority queue's array is not a representation of a min tree (as presented in video 4 on heaps), then it will be **&Theta;(n)**{:.highlighted} to extract the min value and constant time to insert (pushed to back of array) new items to the array.

### P3
>You are given a heap with *n* elements that supports Insert and Extract-Min. Which of the following tasks can you achieve in **O(logn)**{:.highlighted} time?

Assuming the heap is as described in lecture (backed by a min tree represented as an array), then Extract-Min runs at **O(logn)**{:.highlighted} (due to "bubble down" to update the tree after min extraction).  Finding the 5th smallest element will take a constant number of Extract-Min calls, so finding the 5th smallest element will take **O(logn)**{:.highlighted} time.

### P4
>You are given a binary tree (via a pointer to its root) with *n* nodes. As in lecture, let size(x) denote the number of nodes in the subtree rooted at the node x. How much time is necessary and sufficient to compute size(x) for every node x of the tree?

Size(x) will need to be applied to each node, so it will be **&Theta;(n)**{:.highlighted} time.

### P5
>Suppose we relax the third invariant of red-black trees to the property that there are no three reds in a row. That is, if a node and its parent are both red, then both of its children must be black. Call these relaxed red-black trees. Which of the following statements is not true?

For a generic binary search tree (which does not imply a balanced property), simply recoloring the nodes will not be sufficient to convert it to a relaxed red-black tree.  If it were a balanced binary search tree, then it would be sufficient.

---
## Programming Assignment

This programming assignment was trivial with the custom priority queue I implemented from last week's programming assignment.  I needed to add some code to get direct access to elements in the sorted private vector of elements, but otherwise there was no additional code needed to complete this week's programming assignment. 

This week's assignment gave a stream of integers from 1 to 10000 in random order and asked us to find all of the medians of the 1st, 2nd, 3rd, ... n-1, and n integers as they are read from the stream.  After adding the element access functionality to my priority queue, it was trivial to retrieve the successive medians as elements were added.

Below is my short solution:


{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

void solvePart2Week3PA(){
    pqueue<int> Q;
    
    ifstream infile("/home/nathan/Programming/OSSU/Core_Theory/Algorithms-Roughgarden/Part2/Week3/Median.txt");
    
    string line;
    int number;
    int result = 0;
    
    //read in the values into Q, the pqueue
    while (getline(infile, line)){
        number = stoi(line);
        Q.push(number); //O(nlogn)
        
        //add median of current Q, median is found at index half of Q's size.
        //(Round to the floor for even number of values)
        if (Q.size() % 2 == 0) //Even number of values
            result += Q[Q.size()/2 - 1];
        else //Odd number of values
            result += Q[(Q.size()+1)/2 - 1];
        
        //instructions only want the last 4 digits of the sum of medians.
        result = result % 10000;
    }

    cout << result << endl;
    
    infile.close();
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-Pt2Wk4Solution" button-text="Pt 2 Wk 4 Solution" toggle-text=text-capture  footer="End of Solution"%}

Performance could be improved if I used a heap as described in lecture (an array backed binary tree), rather than my binary search backed pqueue.