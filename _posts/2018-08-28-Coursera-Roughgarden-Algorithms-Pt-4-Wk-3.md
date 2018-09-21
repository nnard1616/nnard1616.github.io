---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 4, Week 3"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-28
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
![img]({{ '/assets/images/20180828/Algorithms-pt4-wk3-ex1.png' | relative_url }}){: .center-image }

Let S' be the optimal min-size subset of vertices and S be the subset returned by the greedy algorithm.  Since edges were selected with no common vertices prior to addition to S, none of the edges in S contain common endpoints.  As such, in order for S to be a vertex cover of G one only need one endpoint of each edge that was added to S and this ends up halving the total number of endpoints in S.  Therefore, \|S\| &le; 2 * \|S'\|.&#8718;

### P2
![img]({{ '/assets/images/20180828/Algorithms-pt4-wk3-ex2.png' | relative_url }}){: .center-image }

This problem is taken directly from Cormen et al's Introduction to Algorithms 3rd edition, Theorem 35.4 and Corollary 35.5.  The proof for Theorem 35.4 is rather lengthy and involved so I direct anyone reading this to refer to that text for a full explanation to this problem.  I understand and believe the proofs presented in the text, but I am unable to explain in layman's what it means nor do I want to make the incredible effort of re-proving it.

### P3
![img]({{ '/assets/images/20180828/Algorithms-pt4-wk3-ex3.png' | relative_url }}){: .center-image }

Let C* be an optimal solution pair of sets from the collection of S<sub>1</sub>, S<sub>2</sub>,..., S<sub>m</sub>.  Denote the solution pair as S<sub>i</sub> and S<sub>j</sub>, chosen in that order.  Note then that \|S<sub>i</sub>\| &ge; \|C\*\|/2 because otherwise S<sub>j</sub> would have been picked first instead.  Denote the elements in C\* that are not already in S<sub>i</sub> as x.  The second subset S<sub>j</sub> must then contain half of the elements in x, otherwise either some other subset in the collection, say S<sub>k</sub>, would have been the second selection or x is not actually as large as originally assumed.  If we assume \|S<sub>i</sub>\| equals its lower boundary limit of \|C\*\|/2, then x is also \|C\*\|/2.  And since S<sub>j</sub> must contain half of x's elements, \|S<sub>j</sub>\|'s lower boundary limit will have to be half of \|C\*\|/2, or \|C\*\|/4.  This means the overall lower limit of any sub-optimal solution from the algorithm will have to be \|C\*\|/2 + \|C\*\|/4 = 3\|C\*\|/4.&#8718;

### P4
![img]({{ '/assets/images/20180828/Algorithms-pt4-wk3-ex4.png' | relative_url }}){: .center-image }

This problem is taken directly from Kleinberg/Tardos' Algorithm Design, Theorem 11.3.  Please feel free to refer to that text for more in depth analysis.

Let A = average load across the m machines,  (Sum job times)/m, and B = maximum job time.  Denote T' as the optimal makespan, and note that T' &ge; A and T' &ge; B.  Since the jobs are added as they are read, the last job determines the makespan so we will analyze the last job to be added with t<sub>j</sub> process time.  Denote the last machine to receive the last job as M<sub>i</sub> and its total load after receiving the last job as T<sub>i</sub>.  This means its load prior to receiving job t<sub>j</sub> was T<sub>i</sub> - t<sub>j</sub>.  Additionally, since it was the last machine to receive a job, this means its load was the smallest of all the other machines.  As such, each machine had a load at least T<sub>i</sub> - t<sub>j</sub>.  Summing these all up, we get Sum(T's) &ge; m(T<sub>i</sub> - t<sub>j</sub>), or equivalently: T<sub>i</sub> - t<sub>j</sub> &le; A. Recall that T' &ge; A, so this means that T<sub>i</sub> - t<sub>j</sub> &le; T'.  And since T' &ge; B, we know that T' &ge; t<sub>j</sub> which then implies: T<sub>i</sub> = (T<sub>i</sub> - t<sub>j</sub>) +  t<sub>j</sub> &le; 2T'.  Or just T &le; 2T'. &#8718;

### P5
![img]({{ '/assets/images/20180828/Algorithms-pt4-wk3-ex5.png' | relative_url }}){: .center-image }

Once again, this problem is taken directly from Kleinberg/Tardos' Algorithm Design, Theorem 11.5.  Please feel free to refer to that text for more in depth analysis.

Firstly, if there are more than m jobs, then T' &ge; 2t<sub>m+1</sub>.  Note that the jobs are in decreasing sorted order.  So every job up to the m+1 job will be at least t<sub>m+1</sub>.  Since there's m machines and m+1 jobs, one machine will have two jobs (pigeon hole).  The machine with two jobs will have a total processing time of at least 2t<sub>m+1</sub>.  Since jobs were added in decreasing order, we can then presume T' &ge; 2t<sub>m+1</sub>, or 0.5 T' &ge; t<sub>m+1</sub>

Now we follow a proof similar to the one used in problem 4; let's assume machine M<sub>i</sub> is the next machine to receive a job and it has at least two jobs assigned to it already.  An incoming job, j, will have t<sub>j</sub> &le; t<sub>m+1</sub> &le; 0.5 T'.  Now in following the proof from problem 4, we get T<sub>i</sub> - t<sub>j</sub> &le; T'.  Since  t<sub>j</sub> &le; 0.5 T', we can then conclude T<sub>i</sub> &le; 1.5T'.&#8718;

---
## Programming Assignment

For this programming assignment we look at the TSP again, but this time with a much larger data set (37000+).  This time around the exponential deterministic algorithm from last week will not cut it, so we must make use of an approximate polynomial-time algorithm, which is the following:   

1. Start the tour at the first city.
2. Repeatedly visit the closest city that the tour hasn't visited yet. In case of a tie, go to the closest city with the lowest index. For example, if both the third and fifth cities have the same distance from the first city (and are closer than any other city), then the tour should begin by going from the first city to the third city.
3. Once every city has been visited exactly once, return to the first city to complete the tour.

The data points are already in order by ID number (1, 2, 3,...), but are also, interestingly, in order of increasing x coordinate.  Using the triangle inequality theorem, we can gain a computational advantage.  Instead of having to compare a point to all potential points, we only need to compare points until the minimum recorded distance so far is less than the difference in x coordinates. With this optimization, the following code can run in about 6 seconds:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

void solvePart4Week3Q1(){
    //Open up file of data point data
    string filename = "/home/nathan/Programming/OSSU/Core_Theory/"
            "Algorithms-Roughgarden/Part4/Week3/nn.txt";
    ifstream infile(filename);
    string line;
    
    //read in number of nodes
    getline(infile, line);
    int numberOfNodes = stoi(line);
    
    //create and populate array of data points
    vector<dataPoint*> V;
    
    double x,y;
    int id = 1;
    
    getline(infile, line);
    
    vector<string> coors = split(line, ' ');
        
    x = std::stod(coors[1]);
    y = std::stod(coors[2]);
    
    dataPoint start(id, x, y);
    id++;
    
    start.setPrevious(&start);
    
    //Read in data point values and store in V.
    while (getline(infile, line) ){
        
        vector<string> coors = split(line, ' ');
        
        x = std::stod(coors[1]);
        y = std::stod(coors[2]);
        
        V.push_back(new dataPoint(id, x, y));
        
        id++;
    }
    
    
    cout << setprecision(4) << fixed;
    cout << "Size of V: " << V.size() 
         << " Expected: " << numberOfNodes-1 << endl;
    cout << start.getPrevious()->getID() << endl;
    
    //initialize distance, d
    double totalPathDistance = 0;
    double dx;
    double minSD = 1000000000; //min square distance for current iteration
    double tempMinSD;
    
    int currNodeComp = 0; //number of comparisons made with current node
    int totalComparisons = 0; //counts total comparisons made.
    int nodesProcessed =  0;
    int nextIndex;
    
    
    //while V is not empty
    while(! V.empty()){
        
        currNodeComp=0;
        
        for (int i =0; i < V.size(); i++){
            currNodeComp++;
            tempMinSD = V[i]->squareDistance(*(start.getPrevious()));
            
            if ( tempMinSD < minSD){
                minSD = tempMinSD;
                nextIndex = i;
            }
            
            //datapoints happen to be sorted by x coordinate...
            dx = (V[i]->getX() - start.getPrevious()->getX());
            
            if (minSD < dx*dx){
                break;
            }
        }
        
        totalComparisons += currNodeComp;
        start.setPrevious(V[nextIndex]);
        V.erase(V.begin()+nextIndex);
        totalPathDistance+=pow(minSD,0.5);
        
        nodesProcessed++;
        if (nodesProcessed % 1000 == 0)
            cout << nodesProcessed << ": " <<  V.size() 
                 << " remaining, with distance of " << totalPathDistance 
                 << ", broken at " << currNodeComp << " when the minDN was at: " 
                 << nextIndex << endl;
        minSD = 1000000000;
    }
    
    totalPathDistance += start.distance(*start.getPrevious());
    //print d
    cout << (int)totalPathDistance << endl;
    cout << "total comparisons: " << totalComparisons << endl;
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-tspalgo" button-text="TSP Algorithm" toggle-text=text-capture  footer="End of TSP Algorithm"%}

Strangely, a vector data structure performs better than a list data structure.  I would think everything would be comparable aside from the deletion of elements, which list should perform better with as there's no need to shift contiguous data as with the vector (I think?).  But, vector outperforms the list in this algorithm somehow...


