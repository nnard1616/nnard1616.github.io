---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 4, Week 2"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-25
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
![img]({{ '/assets/images/20180825/Algorithms-pt4-wk2-ex1.png' | relative_url }}){: .center-image }

NP-Complete problems are either all not solvable in polynomial time, or all solvable in polynomial time.  There is no way for there to be some NP-Complete problems to be polynomial-time solvable. 

### P2
![img]({{ '/assets/images/20180825/Algorithms-pt4-wk2-ex2.png' | relative_url }}){: .center-image }

TSP1 reduces to TSP2 and HAM1 reduces to HAM2.  As such, if either TSP2 or HAM2 are polynomial-time solvable, then so are TSP1 and HAM1 respectively.

### P3
![img]({{ '/assets/images/20180825/Algorithms-pt4-wk2-ex3.png' | relative_url }}){: .center-image }

If cycles are allowed when finding the shortest path between s and t with exactly n-1 edges, then a polynomial-time solvable algorithm, such as a Bellman-Ford recurrence, can be used.


### P4
![img]({{ '/assets/images/20180825/Algorithms-pt4-wk2-ex4.png' | relative_url }}){: .center-image }

Vertex covers and independent sets are complements of each other.  If you can solve one in **O(T(n))**{:.highlighted}, then so can the other.  So all three statements are true.

### P5
![img]({{ '/assets/images/20180825/Algorithms-pt4-wk2-ex5.png' | relative_url }}){: .center-image }

Deleting a vertex of a TSP represented by nodes whose edges are the euclidean distances between the nodes cannot increase the cost of the optimal tour.  Since the edges are euclidean distances, they'll all be nonnegative. Removing a vertex will result in one less vertex to visit and so the total distance of the optimal tour can only get smaller.   

---
## Programming Assignment

For this assignment we need to solve the "Traveling Sales-man Problem", more specifically for a graph of 25 points:

<iframe src="https://www.desmos.com/calculator/xln7dfm7no?embed" class="center-image" width="500px" height="500px" style="border: 1px solid #ccc" frameborder="0"></iframe>
<br>

An exponential, though better than pure brute force, algorithm using dynammic programming was presented in lecture:
![img]({{ '/assets/images/20180825/TSPAlgo.png' | relative_url }}){: .center-image }

To see how good/bad the algorithm was, I implemented it exactly as presented in c++.  Here it is below:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

void solvePart4Week2Q1(){
    //Open up file of data point data
    string filename = "/home/nathan/Programming/OSSU/Core_Theory/"
            "Algorithms-Roughgarden/Part4/Week2/tsp.txt";
    ifstream infile(filename);
    string line;
    
    //read in number of nodes
    getline(infile, line);
    int numberOfNodes = stoi(line);
    
    //Array of data points
    vector<dataPoint*> V;
    
    double x,y;
    int id = 0;
    
    //Read in data point values and store in V.
    while (getline(infile, line)){
        
        vector<string> coors = split(line, ' ');
        
        x = std::stod(coors[0]);
        y = std::stod(coors[1]);
        
        V.push_back(new dataPoint(id, x, y));
        
        id++;
    }
    
    
    cout << setprecision(4) << fixed;
    cout << "Size of V: " << V.size() << " Expected: " << numberOfNodes << endl;
    cout << V[0]->getX() << ' ' << V[0]->getY() << endl;
    
    //Create Subsets of V that contain 1, mapped by length of subset.
    
    map< int, vector<vector<dataPoint*>>> subsets = generateChoicesOfS(V);
    cout << "finished making sets" << endl;
    
    //Create mappings from subsets to corresponding index values of Subset by 
    //Destination 2D array, A, which is initialized soon hereafter.
    
    unsigned int c = 0;
    
    //for quickest lookup of subset index, via subset pointer.
    map<vector<dataPoint*>*, int> subsetIndexing;
    
    //for finding index of a difference of two subsets, much slower lookup
    //differences are not easily mapped to a subset pointer (though a mapping 
    //could probably be made...)
    map<vector<dataPoint*>, int> diffIndexing;
    
    for (int m = 1; m <= numberOfNodes; m++){
        for (auto& s : subsets[m]){
            subsetIndexing[&s] = c;
            diffIndexing[s]   = c++;
        }
    }
    
    cout << c << endl;
    cout << "finished making subset to array index map, "
            "now initializing 2D array...." << endl;

    vector<vector<double>> A;
    vector<double> t;
    
    //Initialize A[{1},1] to 0, all other A[S, 1] to +infinity, 
    //where S is in subsets.
    
    for (int i = 0; i < c; i++){
        t.clear();
        for (int j = 0; j < numberOfNodes; j++){
            if (i == 0 && j == 0)
                t.push_back(0);
            else
                t.push_back(INT32_MAX);
        }
        A.push_back(t);
    }
    
    //Initialize A[{1,x}, x] to C_1x as well.
    
    for (auto& subsetOfSize2 : subsets[2])
        A[ subsetIndexing[ &subsetOfSize2 ] ][ subsetOfSize2[1]->getID() ] 
                = subsetOfSize2[0]->distance(*subsetOfSize2[1]);
    
    cout << "Finished initializing A, now running the dynamic programming "
            "algorithm..." << endl;
    
    vector<dataPoint*> diff;
    int subsetIndex, diffIndex;
    
    //cycle through m = 2,3,4,...., n
    for (int m = 3; m <= numberOfNodes; m++){
        cout << m << "..." << endl;
        
        //cycle through each subset S ⊆ {1,2,3,...,n} of size m and containing 1
        for (auto& S : subsets[m]){
    
            //cycle through j in S, skipping j = 1:
            for (int j = 1; j < S.size(); j++){
                
                //cycle through k in S...
                for (int k = 1; k < S.size(); k++){
                    
                    //... skipping k = j (since j = 1 was skipped above, 
                    //k = 1 will be implicitly skipped as well):
                    if ( j != k){
                        
                        diff = S;
                        diff.erase(diff.begin() + j);
                        subsetIndex = subsetIndexing [ &S ];
                        diffIndex   = diffIndexing [ diff ];
                        
                        //A[S, j] = min(A[S, j], A[S-{j}, k] + C_kj)  
                        //note: C_kj is distance from k to j.
                        A[ subsetIndex ][ S[j]->getID() ] 
                        = min( A[ subsetIndex ][ S[j]->getID() ], 
                               A[ diffIndex ][ S[k]->getID() ]
                               + S[k]->distance(*S[j]));
                        
                    }
                }
            }
        }
    }
    
    cout << "Finished the dynamic programming algorithm,"
            " now searching for min value..." << endl;
    
    //initialize result to max value
    int result = INT32_MAX;
    
    //cycle through j = 2,3,...,n
    for (int j = 1; j < numberOfNodes; j++){
        
        //result = min(result, A[V, j] + C_j1])
        result = min(result, (int)(A[c-1][j] + V[j]->distance(*V[0])));
    }
    
    //print result
    cout << "Answer: " << result << endl;
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-tspalgo" button-text="TSP Algorithm" toggle-text=text-capture  footer="End of TSP Algorithm"%}


I created a quick dataPoint data structure for handling the data and computing euclidean distances between points:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

/******************************************************************************/
/******************************************************************************/
/*  dataPoint Header                                                          */
/******************************************************************************/
/******************************************************************************/

#include <math.h>

class dataPoint {
public:
    
    dataPoint(int id, double x, double y);
    dataPoint(const dataPoint& d):dataPoint(d.getID(), d.getX(), d.getY()){}
    
    double distance(dataPoint other);
    int getID() const;
    double getX()  const;
    double getY()  const;
    
    bool operator==(const dataPoint& other);
    
private:
    const int ID;
    const double X, Y;
    
};

/******************************************************************************/
/******************************************************************************/
/*  dataPoint Implementation                                                  */
/******************************************************************************/
/******************************************************************************/

dataPoint::dataPoint(int id, double x, double y) : ID(id), X(x), Y(y){
}

double dataPoint::distance(dataPoint other) {
    return pow( pow((this->X - other.getX()), 2) 
              + pow((this->Y - other.getY()), 2), 0.5);
}

int dataPoint::getID() const {
    return ID;
}

double dataPoint::getX() const {
    return X;
}

double dataPoint::getY() const {
    return Y;
}

bool dataPoint::operator==(const dataPoint& other) {
    return getID() == other.getID();
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-datapoint" button-text="dataPoint" toggle-text=text-capture  footer="End of dataPoint"%}


I also needed a way to generate subsets of a given set.  Doing some research online I found a clever way to do so using bit shifting.  The function that generates my subsets is below:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

template <typename T>
map< int, vector<vector<T>>>  generateChoicesOfS(vector<T> set){
    map< int, vector<vector<T>>> result;
    vector<T> s;
    
    int n = set.size();
    
    for (int i = 0; i < (1<<n); i++){
        s.clear();
        
        //cycle through possible number of elements
        for (int j = 0; j < n; j++){
            
            if ((i & (1<<j)) > 0){
                s.push_back(set[j]);
            }
        }
        
        //skip empty set
        if (s.size() == 0)
            continue;
        
        //only take those that contain first element.
        if (s[0] == set[0]){
            result[s.size()].push_back(s);
        }
    }
    
    return result;
}


{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-subsetgenerator" button-text="Subsets Generator" toggle-text=text-capture  footer="End of Subsets Generator"%}

The map it returns organizes the subsets by size.  So if I want all of the subsets with size of 3, just call the map with 3.  

As the algorithm is **O(n<sup>2</sup>2<sup>n</sup>)**{:.highlighted} the implementation takes an awful long time, around 2 hours of run time. Reading the forums on coursera some students discussed an excellent idea using "Divide and Conquer".  If you split the points into two halves, the algorithm is instant for each case.  Then, using the results of both half-problems, it's only a few checks away from constructing the optimal solution.  I would like to make a more general implementation of this method, I may return to this another day however as I have other things I need to work on.

