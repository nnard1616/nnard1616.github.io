---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 4, Week 1"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-22
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
![img]({{ '/assets/images/20180822/Algorithms-pt4-wk1-ex1.png' | relative_url }}){: .center-image }

If we find all shortest paths from s to every other vertex, the paths will be directed away from s and should provide all unique paths.  There may be overlap between the paths, but you will not get more than one path from s to any arbitrary vertex v.  Therefore, the union of these paths will be a tree in which all edges are directed away form s.

### *P2
![img]({{ '/assets/images/20180822/Algorithms-pt4-wk1-ex2.png' | relative_url }}){: .center-image }

My intuition led me to believe it computes the same thing as the regular BF algorithm, but attained in a different logical pathway.  So it should be the same as the regular BF algorithm, it will compute the correct shortest paths if and only if the graph contains no negative cycles.

Unfortunately, this was a bit of a lucky guess and I don't have a complete understanding of the problem.  Hence I cannot provide a proof for why this answer is correct.

### P3
![img]({{ '/assets/images/20180822/Algorithms-pt4-wk1-ex3.png' | relative_url }}){: .center-image }

Normally BF is **O(mn)**{:.highlighted}, but if each shortest path is at most k edges then the method can be completed in fewer than n iterations.  IE, it can finish in k iterations meaning it would have **O(km)**{:.highlighted} running time.

### P4
![img]({{ '/assets/images/20180822/Algorithms-pt4-wk1-ex4.png' | relative_url }}){: .center-image }

The proposed algorithm will compute the number of paths from u to v for each pair of nodes u and v in N.   Hence, none of the options correctly describe what it computes.

### *P5
![img]({{ '/assets/images/20180822/Algorithms-pt4-wk1-ex5.png' | relative_url }}){: .center-image }

For this, I see why it must be more than n - 1 because in the case of negative cycles in a graph you can get path lengths larger in magnitude than n - 1.  After some experimenting, it seems the upper bound is somewhere between 2<sup>n</sup> and 2<sup>n</sup> /2.  Not really sure how to go about proving this through induction however.

---
## Programming Assignment

Given three graphs, we're asked to find the shortest path of each and to report the shortest of the three.  Concretely and importantly, we're supposed to find the length of the shortest of the shortest paths. IE, we don't need to give the actual path, just the length of it.  

At first I programmed Johnson's algorithm, which uses the Bellman-Ford algorithm in an initial pre processing step and then uses Dijkstra's Algorithm on all vertices of the graph.  This will allow one to compute not just the path lengths but also the actual paths themselves.  However, the assignment only wants the length.  This permits us a shortcut.  The initial Bellman-Ford preprocessing step already computes the shortest path length, so I programmed a simplified Johnson algorithm that just gives the answer after doing one round of Bellman-Ford. Note however, this is only needed if negative weights exist in the graph.  If there are no negative weights, then the shortest path is just whichever edge has the smallest weight.  

Below is the code for my Bellman-Ford Algorithm and my Simplified Johnson Algorithm:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

// returns true if successful, false if there's a negative cycle.
bool DirectedWeightedGraph::bellmanford(int n) {
    bool changesMade = true;
    int iterations = 0;
    
    WeightedNode* curr;
    nodeList[n]->setDistance(0);
    
    int before, after;
    
    // if no changes are made, then we're done, end it.
    // should only need n-1 iterations to get answer, if more iterations are 
    // used, then there's a negative cycle present.
    while (changesMade && iterations < nodes){
        changesMade = false;
        
        //go through each node, i,'s neighbors and update the neighbor distances
        //if i's distance + neighbor's edge weight is less than the neighbor's
        //current distance, update its distance.
        for (auto& i : nodeList){
            curr = i.second;
            
            if (curr->getDistance() == INT32_MAX)
                continue;   // Don't know how to get to this node yet, skip.

            for (auto& neighbor : *curr->getNeighbors()){
                
                before  = neighbor->first->getDistance();
                after   = min(neighbor->first->getDistance(), 
                              curr->getDistance() + neighbor->second);
                
                neighbor->first->setDistance(after);
                
                if (before != after){
                    changesMade = true;
                }
            }
        }
        iterations++;
    }
    
    if (iterations == nodes)
        return false; //negative cycle detected
    else
        return true;
}


{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-bellmanford" button-text="Bellman-Ford" toggle-text=text-capture  footer="End of bellmanford"%}



{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}


//used strictly for finding shortest path length between any two nodes u and v.
//if you want to find all shortest paths, you'll need the full johnson algorithm.
bool DirectedWeightedGraph::johnsonSimplified() {
    
    //if there's no negative weights, then shortest path will be the 
    //smallest edge weight and that was already recorded when reading in the
    //data
    if ( ! negativeEdgeWeightsPresent)
        return true;
    
    // add 0 node with 0 weights to all nodes in nodeList
    WeightedNode* origin;
    origin = new WeightedNode(0);
    for (auto& i : nodeList){
        origin->addNeighbor(new pair<WeightedNode*, int>(i.second, 0));
    }
    
    nodeList[0] = origin;
    nodes++;
    
    //perform bellmanford with 0 node
    if ( ! bellmanford(0))
        return false; //negative cycle detected
    
    // if there are negative weights, then the minimum distance will be the 
    // the min of the distances churned out by bellmanford algorithm.
    for (auto& i : nodeList){
        minPath = min(minPath, i.second->getDistance());
    }
    
    return true;//success
}


{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-simplifiedjohnson" button-text="Simplified Johnson" toggle-text=text-capture  footer="End of simplifiedjohnson"%}