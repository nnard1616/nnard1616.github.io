---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 2, Week 2"
description: My thoughts and solutions for this week's exercises.
date: 2018-07-17
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
>Consider a directed graph with distinct and nonnegative edge lengths and a source vertex *s*. Fix a destination vertex *t*, and assume that the graph contains at least one *s-t* path. Which of the following statements are true? [Check all that apply.]

There is no need for the shortest path *s-t* to include the smallest edge length in the graph.  Same goes for *strictly* excluding the largest edge length.  Though it is possible for the path to contain all of the edges and it is guaranteed that the shortest path will not contain a loop as it is against the definition of "shortest path" to have a loop (it would get truncated).

### P2
>Consider a directed graph *G* with a source vertex *s*, a destination *t*, and nonnegative edge lengths. Under what conditions is the shortest *s-t* path guaranteed to be unique?

There can never be a common sum between two different combinations of distinct powers of 2, thus "when all edges are distinct powers of 2" is the correct answer.

### P3
>Consider a directed graph ***G=(V,E)*** and a source vertex *s* with the following properties: edges that leave the source vertex *s* have arbitrary (possibly negative) lengths; all other edge lengths are nonnegative; and there are no edges from any other vertex to the source *s*. Does Dijkstra's shortest-path algorithm correctly compute shortest-path distances (from *s*) in this graph?

Dijkstra falls apart with negative edge lengths when they appear down the line.  Prior nodes may get processed prior to processing those negative edge lengths and will be closed off prematurely-- thereby missing the true shortest path that utilizes the later negative edge lengths.  

But for this example, the negative edge lengths only appear coming out from the starting node.  Hence, all negative edge lengths are "known" at the start of the algorithm and so it won't miss any prior to closing off subsequent nodes.  Hence, Dijkstra will always work with this example, so long as the negative edge lengths are always on the starting node.


### P4
>Consider a directed graph ***G*** and a source vertex *s*. Suppose ***G*** has some negative edge lengths but no negative cycles, meaning ***G*** does not have a directed cycle in which the sum of the edge lengths is negative. Suppose you run Dijkstra's algorithm on ***G*** (with source *s*). Which of the following statements are true? [Check all that apply.]

It's not impossible to run Dijkstra on a graph with negative edge lengths, there's just no guarantee outright that the computed answer is correct.  Though we can safely say there will be no infinite loops as Dijkstra always terminates in *n-1* iterations.

### P5
>Consider a directed graph ***G*** and a source vertex *s*. Suppose ***G*** contains a negative cycle (a directed cycle in which the sum of the edge lengths is negative) and also a path from *s* to this cycle. Suppose you run Dijkstra's algorithm on ***G*** (with source *s*). Which of the following statements are true? [Check all that apply.]

Again Dijkstra will never loop forever as it always terminates in *n-1* iterations.  As such you can definitely run Dijkstra on this kind of graph, any graph really.  You just have no guarantee that the computed answer is correct.

---
## Programming Assignment

We're asked to implement Dijkstra's algorithm and use it to compute various shortest paths in a given Undirected Graph (I use a directed graph below, but it works just the same).  Apparently there's a couple ways of going about it, but the best approach is to use a heap/priority queue data structure in the implementation.  

Below you will find my implementation using C++.  Click on the buttons to expand each Class's code.  While the C++ STL has a priority_queue data structure, it does not actively keep itself sorted once items within it change (via pointers) and there's no direct member function to force updates and it does not come equipped with iterators.  I tried using the swap member function to pass the nodes between two different priority_queues and other things to force re-sorting of the nodes, but for whatever reason I wasn't getting correct answers.  So I made my own implementation of a priority_queue, backed by a vector and enabled to accept custom Comparator objects for sorting the objects.  I also provided an update() member function which gets called whenever changing my nodes throughout my Dijkstra implementation.  It just forces a re-sorting of the objects via std::sort from the \<algorithm\> library. There's also a neat implmentation of binarysearch using iterators within my priority queue implemenation.



 {% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

/******************************************************************************/
/******************************************************************************/
/*  WeightedNode Header                                                       */
/******************************************************************************/
/******************************************************************************/

#include <map>
#include <set>
#include <iostream>

using namespace std;

class WeightedNode{
public:
    
    //constructors and destructor
    WeightedNode():WeightedNode(0){}
    WeightedNode(int value);
    WeightedNode(const WeightedNode& orig);
    virtual ~WeightedNode();
    
    //Getters & Setters
    unsigned int getValue () const;
    int getDistance () const;
    set<pair<WeightedNode*, int>*>* getNeighbors();
    WeightedNode* getPreviousOptimalNeighbor();
    
    bool isVisited() const;
    bool allNeighborsVisited();
    
    void setValue (int i) ;
    void setVisited( bool b);
    void setDistance(int i);
    
    void setPreviousOptimalNeighbor(WeightedNode* p);
    
    void addNeighbor(pair<WeightedNode*, int>* n);
    
    //Operators
    bool operator==  (const WeightedNode &n1);
    friend ostream& operator<<  (ostream& os, const WeightedNode& n);
    
private:
    unsigned int value;
    bool visited;
    
    set<pair<WeightedNode*, int>*> neighborWeights;
    unsigned int distance = 1000000;
    
    WeightedNode* previousOptimalNeighbor;

};


/******************************************************************************/
/******************************************************************************/
/*  WeightedNode Implementation                                               */
/******************************************************************************/
/******************************************************************************/

/******************************************************************************/
/*  Constructors/Destructor                                                   */
/******************************************************************************/

WeightedNode::WeightedNode(int value){
    setValue(value);
    setVisited(false);
}

WeightedNode::WeightedNode(const WeightedNode& orig) {
}

WeightedNode::~WeightedNode() {
}


/******************************************************************************/
/*  Getters/Setters                                                           */
/******************************************************************************/

unsigned int WeightedNode::getValue() const {
    return value;
}

void WeightedNode::setValue(int i) {
    value = i;
}

set<pair<WeightedNode*, int>*>* WeightedNode::getNeighbors() {
    return &neighborWeights;
}

bool WeightedNode::allNeighborsVisited() {
    for (auto& i : neighborWeights)
        if ( ! i->first->isVisited())
            return false;
    return true;
}

bool WeightedNode::isVisited() const { 
    return visited;
}

void WeightedNode::setVisited( bool b){
    visited = b;
}

int WeightedNode::getDistance() const {
    return distance;
}

void WeightedNode::setDistance(int i) {
    distance = i;
}

WeightedNode* WeightedNode::getPreviousOptimalNeighbor() {
    return previousOptimalNeighbor;
}

void WeightedNode::setPreviousOptimalNeighbor(WeightedNode* p) {
    previousOptimalNeighbor = p;
}

void WeightedNode::addNeighbor(pair<WeightedNode*, int>* n){
    neighborWeights.insert(n);
}


/******************************************************************************/
/*  Operators                                                                 */
/******************************************************************************/

bool WeightedNode::operator==(const WeightedNode& n1) {
    return value == n1.getValue();
}

ostream& operator<<(ostream& os, const WeightedNode& n) {
    os << n.getValue() << ' ';
    for (auto i : n.neighborWeights)
        os << i->first->getValue() << ',' << i->second << ' ';
    
    return os;
}

{% endhighlight %}

{% endcapture %}

{% include widgets/toggle-field.html toggle-name="toggle-weightednode" button-text="WeightedNode" toggle-text=text-capture  footer="End of WeightedNode"%}



{% capture text-capture %}


>C++
{:.filename}
{% highlight c++ linenos %}

/******************************************************************************/
/******************************************************************************/
/*  DirectedWeightedGraph Header                                              */
/******************************************************************************/
/******************************************************************************/

#include "WeightedNode.h"
#include "StaticFunctions.h"
#include "DistanceComparator.h"
#include "../datastructures/pqueue.h"

#include <vector>
#include <fstream>
#include <string>
#include <map>
#include <queue>

using namespace std;

class DirectedWeightedGraph {
public:
    
    //Constructors & Destructor
    DirectedWeightedGraph(string filename);
    DirectedWeightedGraph(const DirectedWeightedGraph& orig);
    virtual ~DirectedWeightedGraph();
    
    //getters and setters
    int getEdges() const;
    int getNodes() const;
    int numberOfEdges();
    
    string getOptimalPath(int n);
    string getAnswer();
    
    map<int, WeightedNode*> getNodeList();
    
    void setAllVisited();
    void setAllNotVisited();
    bool areAllVisited();
    bool areAllNotVisited();
    
    void printPathScores();
    
    //shortest paths solver, n is starting point node
    void dijkstra(int n);
    
    friend ostream& operator<< (ostream& os, const DirectedWeightedGraph& g);
    
private:
    int edges;
    int nodes;
    map<int, WeightedNode*> nodeList;
    
    void readInData(string filename);
};


/******************************************************************************/
/******************************************************************************/
/*  DirectedWeightedGraph Implementation                                      */
/******************************************************************************/
/******************************************************************************/

/******************************************************************************/
/*  Constructors/Destructor                                                   */
/******************************************************************************/

DirectedWeightedGraph::DirectedWeightedGraph(string filename) {
    readInData(filename);
}

DirectedWeightedGraph::DirectedWeightedGraph(const DirectedWeightedGraph& orig){
}

DirectedWeightedGraph::~DirectedWeightedGraph() {
}


/******************************************************************************/
/*  Read & Solver Functions                                                   */
/******************************************************************************/

void DirectedWeightedGraph::readInData(string filename) {
    cout << "reading data..." << endl;
    nodeList.clear();
    nodes = 0;
    edges = 0;
    
    ifstream infile(filename);
    string line;
    
    vector<string> adjacency;
    vector<int> readWeightAndValue;
    
    int nodeValue;
    int nodeWeight;
    
    WeightedNode* parentNode;
    WeightedNode* neighborNode;
    
    pair<WeightedNode*, int>* weightAndValue;
    
    
    while (getline(infile,line)){
        adjacency = split(line, '\t');
        nodeValue = stoi(adjacency.at(0));
        adjacency.erase(adjacency.begin());
        
        //check if node is already in our list of nodes, add it if it isn't
        if (nodeList.find(nodeValue) == nodeList.end()){
            nodeList[nodeValue] = new WeightedNode(nodeValue);
            nodes++;
        }
        
        parentNode = nodeList[nodeValue];
        
        for (auto& i : adjacency){
            
            try{
                readWeightAndValue = strings_to_ints(split(i, ','));
            }catch(invalid_argument){//each line may end with a space, skip it.
                continue;
            }
            
            nodeValue = readWeightAndValue.at(0);
            nodeWeight = readWeightAndValue.at(1);
        
            //check if neighbor is already in our list of nodes, add it if not.
            if (nodeList.find(nodeValue) == nodeList.end()){
                nodeList[nodeValue] = new WeightedNode(nodeValue);
                nodes++;
            }
            
            neighborNode = nodeList[nodeValue];
            
            weightAndValue = 
                         new pair<WeightedNode*, int>(neighborNode, nodeWeight);
            
            edges++;
            parentNode->addNeighbor(weightAndValue);
        }
        adjacency.clear();
    }
    
    infile.close();
    cout << "done reading." << endl;
}

void DirectedWeightedGraph::dijkstra(int n) {
    WeightedNode* start = nodeList[n];
    start->setDistance(0);
    start->setPreviousOptimalNeighbor(nullptr);
    
    //priority queue, Q
    pqueue<WeightedNode*, DistanceComparator> Q;
    
    WeightedNode* current;
    int potentialDistance;
    WeightedNode *j, *k;
    
    //create set of all nodes, store in Q
    for (auto& i : nodeList){
        Q.push(i.second);
    }
    
    while( ! Q.empty() ){
        current = Q.top();
        Q.pop();
        current->setVisited(true);
        
        //Relax neighbors if path distance is less than neighbor's current path
        for (auto& neighbor : *current->getNeighbors()){
            
            //only look at those who are in Q (ie, not visited yet)
            if ( ! neighbor->first->isVisited() ){
                
                //compute distance to this neighbor via current path
                potentialDistance = current->getDistance() + neighbor->second;
                
                //if computed distance is less than neighbor's current path...
                if (potentialDistance < neighbor->first->getDistance()){
                    
                    //...update its path and previous node pointer.
                    neighbor->first->setDistance(potentialDistance);
                    neighbor->first->setPreviousOptimalNeighbor(current);
                    
                    //re-sort Q.
                    Q.update();
                }
            }
        }
    }
}


/******************************************************************************/
/*  Getters/Setters                                                           */
/******************************************************************************/

int DirectedWeightedGraph::getEdges() const {
    return edges;
}

int DirectedWeightedGraph::getNodes() const {
    return nodes;
}

bool DirectedWeightedGraph::areAllVisited() {
    for (auto i : nodeList)
        if ( ! i.second->isVisited())
            return false;
    return true;
}

bool DirectedWeightedGraph::areAllNotVisited() {
    for (auto i : nodeList)
        if (i.second->isVisited())
            return false;
    return true;
}

void DirectedWeightedGraph::setAllNotVisited() {
    for (auto i : nodeList){
        if (i.second->isVisited())
            i.second->setVisited(false);
    }
}

void DirectedWeightedGraph::setAllVisited() {
    for (auto i : nodeList){
        if ( ! i.second->isVisited())
            i.second->setVisited(true);
    }
}

//for debugging purposes
map<int, WeightedNode*> DirectedWeightedGraph::getNodeList() {
    return nodeList;
}

void DirectedWeightedGraph::printPathScores() {
    for (auto& i : nodeList){
        cout << i.second->getValue() << ' ' << i.second->getDistance() << endl;
    }
}

//assumes dijkstra has already been run.
string DirectedWeightedGraph::getOptimalPath(int n) {
    string result;
    WeightedNode* i = nodeList[n];
    
    while (i != nullptr){
        result =  to_string(i->getValue()) + " " + result;
        i = i->getPreviousOptimalNeighbor();
    }
    
    return result;
}

//assumes dijkstra has already been run.
string DirectedWeightedGraph::getAnswer() {
    string result;
    int nodes [10] = {7,37,59,82,99,115,133,165,188,197};
    
    for (int i : nodes){
        result += to_string(nodeList[i]->getDistance()) + ',';
    }
    
    result.pop_back();
    
    return result;
}

//for debugging purposes
int DirectedWeightedGraph::numberOfEdges() {
    int result = 0;
    for (auto& i : nodeList){
        result += i.second->getNeighbors()->size();
    }
    return result;
}


/******************************************************************************/
/*  Operators                                                                 */
/******************************************************************************/

ostream& operator<< (ostream& os, const DirectedWeightedGraph& g ){
    
    for (auto pair : g.nodeList){
        os << *pair.second << endl;
    }
    return os;
}


{% endhighlight %}

{% endcapture %}

{% include widgets/toggle-field.html toggle-name="toggle-directedweightedgraph" button-text="DirectedWeightedGraph" toggle-text=text-capture  footer="End of DirectedWeightedGraph"%}





{% capture text-capture %}
>C++
{:.filename}
{% highlight c++ linenos %}

/******************************************************************************/
/******************************************************************************/
/*  pqueue Header                                                             */
/******************************************************************************/
/******************************************************************************/

#include <vector>
#include <iostream>
#include <iterator>
#include <algorithm>

using namespace std;

//priority queue with an updater function should an object change.
template <class Comparable, class Compare = std::less<Comparable> >
class pqueue {
public:
    
    //constructors
    pqueue();
    pqueue(vector<Comparable> objects);
    
    //getters
    vector<Comparable> getObjects();
    bool empty();
    int size();
    Comparable top();
    
    //container operations
    void push(Comparable object);
    void pop();
    void update();
    
    //needed by push operation
    typename vector<Comparable>::iterator binarySearch(Comparable object);
    
private:
    
    vector<Comparable> objects;
};

#include "pqueue.tpp"


/******************************************************************************/
/******************************************************************************/
/*  pqueue Implementation                                                     */
/******************************************************************************/
/******************************************************************************/

/******************************************************************************/
/*  Constructors                                                              */
/******************************************************************************/

template <class Comparable, class Compare> 
pqueue<Comparable, Compare>::pqueue() {
}

template <class Comparable, class Compare> 
pqueue<Comparable, Compare>::pqueue(vector<Comparable> objects) {
    this->objects.swap(objects);
}


/******************************************************************************/
/*  Getters                                                                   */
/******************************************************************************/

template <class Comparable, class Compare> 
Comparable pqueue<Comparable, Compare>::top(){
    return objects.front();
}

template <class Comparable, class Compare> 
vector<Comparable> pqueue<Comparable, Compare>::getObjects() {
    return objects;
}

template<class Comparable, class Compare>
bool pqueue<Comparable, Compare>::empty() {
    return objects.empty();
}

template<class Comparable, class Compare>
int pqueue<Comparable, Compare>::size() {
    return objects.size();
}


/******************************************************************************/
/*  Container Operations                                                      */
/******************************************************************************/

template <class Comparable, class Compare> 
void pqueue<Comparable, Compare>::pop() {
    objects.erase(objects.begin());
}

template <class Comparable, class Compare> 
void pqueue<Comparable, Compare>::push(Comparable object) {
    objects.insert(binarySearch(object), object);
}

template<class Comparable, class Compare>
void pqueue<Comparable, Compare>::update() {
    std::sort(objects.begin(), objects.end(), Compare());
}

template <class Comparable, class Compare> 
typename vector<Comparable>::iterator pqueue<Comparable, Compare>::
binarySearch(Comparable object) {
    
    typename vector<Comparable>::iterator low  = objects.begin();
    typename vector<Comparable>::iterator high = objects.end() - 1;
    typename vector<Comparable>::iterator mid  = objects.begin();
    
    //initialize comparator
    Compare compare = Compare();
    
    // if array empty
    if (objects.empty())
        return objects.begin();
    
    // if object is smaller than first item in array
    if (compare(object, *low))
        return objects.begin();
    
    // if object is greater than last item in array
    if (compare(*high, object))
        return objects.end();
    
    // object is between low and high bounds, search for it...
    while (low <= high){
        
        mid = low + std::distance(low, high)/2 ;
        
        if (compare(*mid, object))
            low = mid+1;
        else if (compare(object, *mid))
            high = mid-1;
        else
            return mid;
        
    }
    
    // object was not present in array, 
    // return iterator where it should be inserted.
    return low;
}
{% endhighlight %}

{% endcapture %}

{% include widgets/toggle-field.html toggle-name="toggle-pqueue" button-text="pqueue" toggle-text=text-capture  footer="End of pqueue"%}



This here is the custom Comparator object for sorting the nodes by path distance.
{% capture text-capture %}
>C++
{:.filename}
{% highlight c++ linenos %}

struct DistanceComparator {
    bool operator() (const WeightedNode* x, const WeightedNode* y ) {
        return x->getDistance() < y->getDistance();
    }
    typedef bool result_type;
};

{% endhighlight %}

{% endcapture %}

{% include widgets/toggle-field.html toggle-name="toggle-DistanceComparator" button-text="DistanceComparator" toggle-text=text-capture  footer="End of DistanceComparator"%}
