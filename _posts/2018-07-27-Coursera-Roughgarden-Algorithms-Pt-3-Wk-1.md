---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 3, Week 1"
description: My thoughts and solutions for this week's exercises.
date: 2018-07-27
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
>We are given as input a set of nn requests (e.g., for the use of an auditorium), with a known start time *s<sub>i</sub>* and finish time *t<sub>i</sub>* for each request *i*. Assume that all start and finish times are distinct. Two requests conflict if they overlap in time --- if one of them starts between the start and finish times of the other. Our goal is to select a maximum-cardinality subset of the given requests that contains no conflicts. (For example, given three requests consuming the intervals [0,3], [2,5], and [4,7], we want to return the first and third requests.) We aim to design a greedy algorithm for this problem with the following form: At each iteration we select a new request *i*, including it in the solution-so-far and deleting from future consideration all requests that conflict with *i*.
>
>Which of the following greedy rules is guaranteed to always compute an optimal solution?

By selecting the interval with earliest finish time, we are guaranteed the fewest conflicts with the other intervals resulting in a minimal elimination of intervals at each iteration.  Since a minimal number of intervals are eliminated at each iteration we are guaranteed a maximum-cardinality.

### P2
>We are given as input a set of *n* jobs, where job *j* has a processing time *p<sub>j</sub>* and a deadline *d<sub>j</sub>*. Recall the definition of completion times *C<sub>j</sub>* from the video lectures. Given a schedule (i.e., an ordering of the jobs), we define the lateness *l<sub>j</sub>* of job *j* as the amount of time *C<sub>j</sub> - d<sub>j</sub>* after its deadline that the job completes, or as 0 if *C<sub>j</sub>* &le; *d<sub>j</sub>*. Our goal is to minimize the maximum lateness, *max<sub>j</sub>* *l<sub>j</sub>*.
>
>Which of the following greedy rules produces an ordering that minimizes the maximum lateness? You can assume that all processing times and deadlines are distinct.

We can get a minimal lateness by ordering the jobs by increasing order of deadline. Here is a proof by exchange for why:

Suppose the jobs are ordered in decreasing order of deadline.  Pick three in order arbitrary jobs: *i*, *j*, and *k*. We see that *l<sub>j</sub>* = *C<sub>j</sub>* - *d<sub>j</sub>* = *C<sub>i</sub>* + *p<sub>j</sub>* - *d<sub>j</sub>* and *l<sub>k</sub>* = *C<sub>k</sub>* - *d<sub>k</sub>* = *C<sub>i</sub>* + *p<sub>j</sub>* + *p<sub>k</sub>* - *d<sub>k</sub>*.  Since *j* and *k* are in order, *d<sub>j</sub>* &gt; *d<sub>k</sub>*.  This means that :

![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex2-1.svg' | relative_url }}){: .latexequation } 

Now we switch jobs *j* and *k*, and let's denote their lateness symbols as *l<sub>j</sub>*\` and *l<sub>k</sub>*\`.  Doing the same analysis as above yields an uncertain comparison, ie there's no way to determine which is bigger between *l<sub>j</sub>*\` and *l<sub>k</sub>*\`.  However, comparing *l<sub>j</sub>*\` and *l<sub>k</sub>*\` to  *l<sub>k</sub>* reveals they are both smaller than *l<sub>k</sub>* :

![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex2-2.svg' | relative_url }}){: .latexequation }

![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex2-3.svg' | relative_url }}){: .latexequation } 
Thus reversing jobs *j* and *k* resulted in lateness values smaller than the larger lateness value prior to their reversal.  Since *j* and *k* are arbitrary, this will be true for any pair of jobs listed in decreasing order of deadline and we can therefore conclude that *max<sub>j</sub>* *l<sub>j</sub>* is minimized when jobs are listed in increasing order of deadline.**&#8718;**{: .highlighted}


### P3
![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex3.png' | relative_url }}){: .center-image }

>Algorithm A
{:.filename}
{% highlight Pseudocode linenos %}
While T has at least one vertex:
  If T has no edges: 
    halt and output "T has no perfect matching."
  Else:
    Let v be a vertex of T with maximum degree.  <--
    Choose an arbitrary edge e incident to v.
    Delete e and its two endpoints from T.
[end of while loop]
Halt and output "T has a perfect matching."
{% endhighlight %}

>Algorithm B
{:.filename}
{% highlight Pseudocode linenos %}
While T has at least one vertex:
  If T has no edges: 
    halt and output "T has no perfect matching."
  Else:
    Let v be a vertex of T with minimum non-zero degree.  <--
    Choose an arbitrary edge e incident to v.
    Delete e and its two endpoints from T.
[end of while loop]
Halt and output "T has a perfect matching."
{% endhighlight %}

A counter example exists for Algorithm A, a three hop path.  Algorithm B will work every time.  A mini proof by induction (sort of):

Assume that a tree-graph, **T**{:.highlighted}, has a perfect matching.  The smallest possible tree-graph with a perfect matching is that of a n = 2 and m = 1 graph with both vertices attached. **T**{:.highlighted}, by the definition of trees, must have at least one vertex with degree 1.  With each iteration of the algorithm, a vertex of degree 1 will always be selected as each resulting graph will be composed of one or more trees.  This will continue until two vertices joined by a vertex remain and the algorithm will complete with an affirmative answer.

Suppose another tree-graph, **T'**{:.highlighted}, does not have a perfect matching.  The smallest possible tree-graph with no perfect matching is that of n = 1 and m = 0, ie a single vertex with no edges.  As the algorithm iterates, it will eventually generate at least one zero degree vertex.  At this point the algorithm can stop or go until no more edges are left as stated above, and return a negative answer.**&#8718;**{: .highlighted}


### P4
![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex4.png' | relative_url }}){: .center-image }

A minimum spanning tree will always have n - 1 edges.  So if there exists any other spanning tree then increasing edge costs by 1 will be the same increase as for any other spanning tree.  Thus the minimum spanning tree will be unchanged as there's no chance for another spanning tree to gain a smaller total edge cost.  

For the shortest path, however, the number of edges is not guaranteed.  If there's another path with fewer edges than the current shortest path, then it will receive a smaller increase to its total edge cost and could end up becoming the new shortest path.  

Therefore, the minimum spanning tree will be unchanged and the shortest path could change depending on the tree.

### P5
![img]({{ '/assets/images/20180729/Algorithms-pt3-wk1-ex5.png' | relative_url }}){: .center-image }

Since H is connected and contains all edges from G that are incident on H's vertices, then the minimum spanning tree of G is contained within any H.

Additionally, the cut property implies that cuts in G correspond to cuts in H with only fewer crossing edges.

---
## Programming Assignment

### Parts 1&2

Given a list of jobs containing a weight and a length, implement algorithm to order the jobs in decreasing order of the difference (weight - length).  Then implement the algorithm to sort the jobs in decreasing order of the ratio (weight/length).  

Once again, using my personal [pqueue data structure from a previous a post]({% post_url 2018-07-17-Coursera-Roughgarden-Algorithms-Pt-2-Wk-2 %}), it was trivial to sort the jobs in either of these two methods.  I just needed to define new comparator structs to feed my pqueue, which are defined below:


{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}
template <class T> struct BadJobsComparator {
    bool operator() (const pair<T, T> x, 
                     const pair<T, T> y ) {
        int firstDifference = x.first - x.second;
        int secondDifference = y.first - y.second;
        
        if (firstDifference != secondDifference)
            return firstDifference > secondDifference;
        else
            return x.first > y.first;
    }
    typedef bool result_type;
};

template <class T> struct GoodJobsComparator {
    bool operator() (const pair<T, T> x, 
                     const pair<T, T> y ) {
        return x.first/(x.second + 0.0) > y.first/(y.second + 0.0);
    }
    typedef bool result_type;
};

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-jobsComparators" button-text="Jobs Comparators" toggle-text=text-capture  footer="End of jobsComparators"%}

Both implementations run in a few milliseconds.

### Part 3

Here we implement Prim's Algorithm for computing the minimum spanning tree.  Using previous graph structures and my pqueue data structure I was able to make quick work of it again.

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}
int UndirectedWeightedGraph::prim () {
    PrimEdge* nextPath = nullptr;
    PrimNode* currentNode = unvisitdedNodeList.begin()->second;
    
    while ( ! unvisitdedNodeList.empty()) {
        
        //erase current node from unvisitedNodeList map structure.
        unvisitdedNodeList.erase(currentNode->getValue());
        
        //add current node to visitedNodeList map structure.
        visitedNodeList[currentNode->getValue()] = currentNode;
        
        //mark current node as visited
        currentNode->setVisited(true);
        
        //have pqueue structures re-order edges.
        edgeList.update();
        minCostEdgeList.update();
        
        //get next path to add to our minCostEdgeList, a pqueue
        nextPath = edgeList.top();
        
        //prevent duplicate of least cost edge
        if (nextPath->oneVisited())
            
            //add nextPath to MST
            minCostEdgeList.push(nextPath);
        
        //assign currentNode to the other unvisited node of this edge
        if ( ! nextPath->getFirst()->isVisited())
            currentNode = nextPath->getFirst();
        else
            currentNode = nextPath->getSecond();
    }
    return sumMin();
}
{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-prim" button-text="Prim's Algorithm" toggle-text=text-capture  footer="End of Prim's Algorithm"%}

It runs in milliseconds.  Ideally I should have defined a new comparator for my pqueue structures that hold the edges, but for whatever reason I was having weird compile errors.  So instead I overloaded the < operator of my Edge class:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}
bool PrimEdge::operator<( PrimEdge& e) {
    if (oneVisited() && ! e.oneVisited())
        return true;
    if (! oneVisited() && e.oneVisited())
        return false;
    return getWeight() < e.getWeight();
}
{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-edgelessthan" button-text="Edge Less Than" toggle-text=text-capture  footer="End of Edge Less Than"%}

This allows the pqueue to sort the edges with those with strictly one visited node up front and in order of increasing weight in the list.  This will ensure that only those that are available for picking (ie, edges with only one visited node) are at the front.
