---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 2, Week 1"
description: My thoughts and solutions for this week's exercises.
date: 2018-07-09
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
>Given an adjacency-list representation of a directed graph, where each vertex maintains an array of its outgoing edges (but *not* its incoming edges), how long does it take, in the worst case, to compute the in-degree of a given vertex? As usual, we use *n* and *m* to denote the number of vertices and edges, respectively, of the given graph. Also, let *k* denote the maximum in-degree of a vertex. (Recall that the in-degree of a vertex is the number of edges that enter it.)

We need to examine, at worst, all of the edges to see if any go into a given node.  Thus **&Theta;(m)**{:.highlighted} is the correct answer.

### P2
>Consider the following problem: given an undirected graph ***G*** with *n* vertices and *m* edges, and two vertices *s* and *t*, does there exist at least one *s-t* path?
>
>If ***G*** is given in its adjacency list representation, then the above problem can be solved in **O(m+n)** time, using BFS or DFS. (Make sure you see why this is true.)
>
>Suppose instead that ***G*** is given in its adjacency *matrix* representation. What running time is required, in the worst case, to solve the computational problem stated above? (Assume that ***G*** has no parallel edges.)

In the adjaceny list version, there are *m+n* entries that would need to be examined.  But in the matrix version, there are now *n<sup>2</sup>* entries that could all be potentially examined.  Hence it is **&Theta;(n<sup>2</sup>)**{:.highlighted}.

### P3
>This problem explores the relationship between two definitions about graph distances. In this problem, we consider only graphs that are undirected and connected. The diameter of a graph is the maximum, over all choices of vertices *s* and *t*, of the shortest-path distance between *s* and *t*. (Recall the shortest-path distance between *s* and *t* is the fewest number of edges in an *s-t* path.)
>
>Next, for a vertex *s*, let *l(s)* denote the maximum, over all vertices *t*, of the shortest-path distance between *s* and *t*. The radius of a graph is the minimum of l(s)l(s) over all choices of the vertex *s*.
>
>Which of the following inequalities always hold (i.e., in every undirected connected graph) for the radius *r* and the diameter *d*? [Select all that apply.]

By the definitions, it stands to reason that the radius can be as large as the diameter.  The diameter must be less than or equal to twice the radius because any central vertex of a graph can be at most the radius length from any pair of periphery vertices. 

### P4
>Consider our algorithm for computing a topological ordering that is based on depth-first search (i.e., NOT the "straightforward solution"). Suppose we run this algorithm on a graph ***G*** that is NOT directed acyclic. Obviously it won't compute a topological order (since none exist). Does it compute an ordering that minimizes the number of edges that go backward?
>
>For example, consider the four-node graph with the six directed edges (s,v),(s,w),(v,w),(v,t),(w,t),(t,s). Suppose the vertices are ordered s,v,w,t. Then there is one backwards arc, the (t,s) arc. No ordering of the vertices has zero backwards arcs, and some have more than one.

It depends on where you start the DFS.  So sometimes yes, sometimes no.

### P5
>On adding one extra edge to a directed graph ***G***, the number of strongly connected components...?

Anything is possible upon adding an edge.  Hence all of the answer choices with strong language are wrong. 

## Programming Assignment

We're asked to implement the Kosaraju's Two-Pass Algorithm for finding Strongly Connected Components (SCC) in a directed graph.  An SCC is any collection of nodes in a graph that one can traverse from i to j within that cluster alone.  Kosaraju's Two-Pass Algortihm can be used to find all SCCs in a graph.  Below is my implementation (Node first, then DirectedGraph) of the algorithm, which will print the top 5 most connected SCCs in a given graph.  For this project I switched to C++ as cloning objects is very frustrating in Java.  For the programming assignment it runs in about 39 seconds.

Note that there's two different implementations for the DFS method in the DirectedGraph class.  The commented out version uses recursion, but if used on the Programming Assignment the program will crash due to a stack overflow.  The second version uses a stack data structure and iteration instead to circumvent this limitation.  



>C++
{:.filename}
{% highlight java linenos %}

/******************************************************************************/
/******************************************************************************/
/*  Node Class Implementation                                                 */
/******************************************************************************/
/******************************************************************************/

//for sorting Node pointers in the neighbors set.
template <class T> struct ptrLess {
    bool operator() (const T* x, const T* y) const {return *x<*y;}
    typedef T first_argument_type;
    typedef T second_argument_type;
    typedef bool result_type;
};

class Node{
public:

    //Constructors & Destructor
    Node();
    Node(int value);
    Node(const Node& orig);
    virtual ~Node();
    
    //Getters & Setters
    int getValue () const;
    set<Node*, ptrLess<Node>>* getNeighbors();
    
    bool isVisited() const;
    bool allNeighborsVisited();
    
    void setValue (int i) ;
    void setVisited( bool b);
    
    void addNeighbor(Node* n) ;
    
    //Operators
    bool operator<   (const Node &n1) const;
    bool operator==  (const Node &n1);
    bool operator!=  (const Node &n1);
    friend ostream& operator<<  (ostream& os, const Node& n);
    
private:
    
    //Attributes
    int value;
    bool visited;
    set<Node*, ptrLess<Node>> neighbors;
};

/******************************************************************************/
/*  Constructors/Destructor                                                   */
/******************************************************************************/

Node::Node() {
    value = 0;
    visited = false;
}

Node::Node(int value){
    this->value = value;
}

Node::Node(const Node& orig) {
    this->value = orig.getValue();
}

Node::~Node() {
}


/******************************************************************************/
/*  Neighbor Methods                                                          */
/******************************************************************************/

void Node::addNeighbor(Node* n)  {
    neighbors.insert(n);
}


/******************************************************************************/
/*  Getters/Setters                                                           */
/******************************************************************************/

int Node::getValue() const {
    return value;
}

void Node::setValue(int i) {
    value = i;
}

set<Node*, ptrLess<Node> >* Node::getNeighbors() {
    return &neighbors;
}

bool Node::allNeighborsVisited() {
    for (auto& i : neighbors)
        if ( ! i->isVisited())
            return false;
    return true;
}

bool Node::isVisited() const { 
    return visited;
}

void Node::setVisited( bool b){
    visited = b;
}


/******************************************************************************/
/*  Operators                                                                 */
/******************************************************************************/

bool Node::operator==(const Node& n1) {
    return n1.getValue() == getValue();
}

bool Node::operator!=(const Node& n1) {
    return ! (n1.getValue() == getValue());
}

bool Node::operator<(const Node& n1) const {
    return getValue() < n1.getValue();
}

ostream& operator<<(ostream& os, const Node& n) {
    os << n.value << ' ';
    
    for (auto i : n.neighbors)
        os << i->value << ' ';
    
    return os;
}


/******************************************************************************/
/******************************************************************************/
/*  DirectedGraph Class Implementation                                        */
/******************************************************************************/
/******************************************************************************/

class DirectedGraph {
public:
    //Constructors & Destructor
    DirectedGraph(string filename);
    DirectedGraph(const DirectedGraph& orig);
    virtual ~DirectedGraph();
    
    int getEdges() const;
    int getNodes() const;
    
    void setAllVisited();
    void setAllNotVisited();
    bool areAllVisited();
    bool areAllNotVisited();

    
    void readInData(string filename);
    void findSCCs();
    
    friend ostream& operator<< (ostream& os, const DirectedGraph& g);
    
private:
    int edges;
    int nodes;
    map<int, Node*> nodeList;
    map<int, int> nToFinishTime;
    
    map<int, Node*> reverseArcsAndTransform(const map<int, Node*>& al);
    void DFS(Node* n, int& counter);
    void DFSOnOriginal();
    priority_queue<int> DFSOnReversedTransform();
};

/******************************************************************************/
/*  Constructors/Destructor                                                   */
/******************************************************************************/

DirectedGraph::DirectedGraph(string filename) {
    readInData(filename);
}

DirectedGraph::DirectedGraph(const DirectedGraph& orig) {
    edges = orig.edges;
    nodes = orig.nodes;
    nodeList = orig.nodeList;
}

DirectedGraph::~DirectedGraph() {
}


/******************************************************************************/
/*  Public Graph Methods                                                      */
/******************************************************************************/

void DirectedGraph::readInData(string filename) {
    cout << "reading data..." << endl;
    nodeList.clear();
    nToFinishTime.clear();
    nodes = 0;
    edges = 0;
    
    ifstream infile(filename);
    string line;
    
    vector<int> pair;
    
    while (getline(infile,line)){
        pair = strings_to_ints(split(line, ' '));
        
        int mInt = pair.at(0);
        int nInt = pair.at(1);
        
        
        if (nodeList.find(mInt) == nodeList.end()){
            nodeList[mInt] = new Node(mInt);
            nodes++;
        }
        
        if (nodeList.find(nInt) == nodeList.end()){
            nodeList[nInt] = new Node(nInt);
            nodes++;
        }
        
        nodeList[mInt]->addNeighbor(nodeList[nInt]);
        edges++;
        
        pair.clear();
    }
    
    
    cout << "Data read. " << endl << "Nodes: "  << nodes 
                                  << " Edges: " << edges << endl;
    
}

void DirectedGraph::findSCCs() {
    cout << "Performing DFS on original..." << endl;
    
    //side affect: 'nToFinishTime' contains mapping from original nodes to 
    //DFS finish times.
    DFSOnOriginal();
    
    cout << "Done. Performing reverse and map..." << endl;
    nodeList = reverseArcsAndTransform(nodeList);
    
    cout << "Done. Performing DFS on reversed transform..." << endl;
    priority_queue<int> result = DFSOnReversedTransform();
     
    cout << "Done.  Here are the top 5 results: " << endl;
    for(int i = 0; i < 5; i ++){
        if (result.size() > 0){
            cout << result.top() << ' ';
            result.pop();
        }else{
            cout << 0 << ' ';
        }
    }
    cout << endl;
}


/******************************************************************************/
/*  Private Graph Methods                                                     */
/******************************************************************************/

//Reverses the arcs in graph, and also uses 'nToFinishTime' to map to the
//respective finish time from DFS on original graph.
map<int, Node*> DirectedGraph::reverseArcsAndTransform(
                                                    const map<int, Node*>& al) {
    
    //reversed, transformed adjacency list.
    map<int, Node*> rtal;
    
    for (auto pair : al){
        
        //transform first node
        int mInt = nToFinishTime[pair.first];
        
        for ( auto i : *pair.second->getNeighbors()){
            
            //transform second node
            int nInt = nToFinishTime[i->getValue()];
            
            if (rtal.find(mInt) == rtal.end())
                rtal[mInt] = new Node(mInt);

            if (rtal.find(nInt) == rtal.end())
                rtal[nInt] = new Node(nInt);
            
            //reverse arc
            rtal[nInt]->addNeighbor(rtal[mInt]);
            
            //ensure visited parameter is set to false.
            rtal[mInt]->setVisited(false);
            rtal[nInt]->setVisited(false);
        }
    }
    
    return rtal;
}

//with recursion
//void DirectedGraph::DFS(Node* n, int& counter) {
//    n->setVisited(true);
//    for (auto& neighbor : *n->getNeighbors()){
//        if ( ! neighbor->isVisited()){
//            DFS(neighbor, counter);
//        }
//    }
//    
//    if (nToFinishTime.size() < nodes)
//        if (nToFinishTime.find(n->getValue()) == nToFinishTime.end())
//            nToFinishTime[n->getValue()] = counter;
//    counter++;
//}

//without recursion
void DirectedGraph::DFS(Node* n, int& counter) {
    stack<Node*> path;
    n->setVisited(true);
    path.push(n);
    
    while (! path.empty()){
        for (auto& i: *path.top()->getNeighbors()){
            if ( ! i->isVisited()){
                i->setVisited(true);
                path.push(i);
                break;//move to neighbor
            }
        }
        
        //if current node has no unvisited neighbors, start backtracking.
        if (path.top()->allNeighborsVisited()){
            
            //this portion is only needed for DFS on original graph.  It will 
            //be skipped over in DFS on reversed transformed graph.
            if (nToFinishTime.size() < nodes)
                if (nToFinishTime.find(path.top()->getValue()) == 
                    nToFinishTime.end())
                    nToFinishTime[path.top()->getValue()] = counter;
            
            counter++;
            path.pop();
        }
    }
}

void DirectedGraph::DFSOnOriginal() {
    int counter = 1;
    int node = nodes;
    
    while (counter <= nodes){
        
        //start at highest node, jump to next highest unvisited node after
        //a round of DFS.
        while (nodeList[node]->isVisited())
            node--;
        
        DFS(nodeList[node], counter);
    }
}

priority_queue<int> DirectedGraph::DFSOnReversedTransform() {
    priority_queue<int> SCCs;
    int prevCounter;
    int counter = 1;
    int node = nodes;
    
    while (counter <= nodes){
        
        prevCounter = counter;
        
        //start at highest node, jump to next highest unvisited node after
        //a round of DFS.
        while (nodeList[node]->isVisited())
            node--;
        
        DFS(nodeList[node], counter);
        
        SCCs.push(counter - prevCounter);
        
    }
    return SCCs;
}


/******************************************************************************/
/*  Getters/Setters                                                           */
/******************************************************************************/

int DirectedGraph::getEdges() const {
    return edges;
}

int DirectedGraph::getNodes() const {
    return nodes;
}

bool DirectedGraph::areAllVisited() {
    for (auto i : nodeList)
        if ( ! i.second->isVisited())
            return false;
    return true;
}

bool DirectedGraph::areAllNotVisited() {
    for (auto i : nodeList)
        if (i.second->isVisited())
            return false;
    return true;
}

void DirectedGraph::setAllNotVisited() {
    for (auto i : nodeList){
        if (i.second->isVisited())
            i.second->setVisited(false);
    }
}

void DirectedGraph::setAllVisited() {
    for (auto i : nodeList){
        if ( ! i.second->isVisited())
            i.second->setVisited(true);
    }
}


/******************************************************************************/
/*  Operators                                                                 */
/******************************************************************************/

ostream& operator<< (ostream& os, const DirectedGraph& g ){
    for (auto pair : g.nodeList){
        os << *pair.second << endl;
    }
    return os;
}
{% endhighlight %}