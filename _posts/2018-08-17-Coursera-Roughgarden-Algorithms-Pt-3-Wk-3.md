---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 3, Week 3"
description: My thoughts and solutions for this week's exercises.
date: 2018-08-17
comments: true
tags:
 - coursera
 - roughgarden
 - algorithms
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

## Quiz

### P1
![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex1.png' | relative_url }}){: .center-image }

Just apply Huffman's algorthim and you'll get a tree with depth of 3.  Then it's just a matter of doing the expectation calculation for how many bits per character and multiplying by 1000.

### P2
![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex2.png' | relative_url }}){: .center-image }

It is possible that an encoding tree can attain a depth of n-1, such as one of the early examples in lecture.

### P3
![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex3.png' | relative_url }}){: .center-image }

If the most frequent letter has frequency less than 0.33, then all letters will be coded with at least two bits.  This works out because every letter will undergo at least 2 merges.  The most frequent letter has to undergo two merges because right before its first merge there will be two other branches where at least one of the branches has a cumulative frequency at least (1-.33)/2 > 0.33.  So it will get merged with one branch, then the other for a total of two merges and resulting in every letter with at least two bits.  Problem 1 is an example.

### P4
![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex4.png' | relative_url }}){: .center-image }

The dynamic programming algorithm, being a recursive algorithm that depends on just the previous two subproblem solutions, can guarantee that once a vertex is excluded twice in a row it will never be in any subsequent subproblem solution.
 
### P5
![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex5.png' | relative_url }}){: .center-image }

The proposed algorithm should work for all path graphs and tree graphs with an efficiency of **O(n)**{:.highlighted}.  All general graphs aren't guaranteed to work out.  Consider a pentagon with a central vertex that is connected to all outter vertices.  If you were to arbitrarily pick any vertex, say the central vertex, then you won't get the same output for every other starting vertex.  (starting with central vertex yields an empty set for max weight independent set)

---
## Programming Assignments

### PA 1&2

This week's programming assignments were pretty straightforward.  Once again I found my beloved [pqueue]({% post_url 2018-07-17-Coursera-Roughgarden-Algorithms-Pt-2-Wk-2 %}) to be very helpful. For the first two questions regarding Huffman Codes, I needed to write a new data structure for the Weighted Binary Tree used to represent the Huffman encoding, whose implementation is below:

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}
/******************************************************************************/
/******************************************************************************/
/*  WeightedTreeNode Header                                                   */
/******************************************************************************/
/******************************************************************************/

#include "WeightComparator.h"
#include <iostream>
#include <vector>

typedef unsigned long long ull;

class WeightedTreeNode {
public:
    WeightedTreeNode();
    WeightedTreeNode(int value, ull weight);
    //next constructor is for creating a fork node
    WeightedTreeNode(int value, WeightedTreeNode* n1, WeightedTreeNode* n2);
    WeightedTreeNode(int value, ull weight, WeightedTreeNode* n1, WeightedTreeNode* n2);
    
    ull getWeight();
    WeightedTreeNode* getLeft();
    WeightedTreeNode* getRght();
    int getValue();
    int getDepth() const;
    void incDepths();
    void incDepth();
    
    friend ostream& operator<< (ostream& os, const WeightedTreeNode& w);
    friend ostream& operator<< (ostream& os, const WeightedTreeNode* w);
private:
    int value;
    int depth = 0; //keeps track of how deep into tree the node is
    ull weight;
    WeightedTreeNode* left = nullptr;
    WeightedTreeNode* rght = nullptr;
};

/******************************************************************************/
/******************************************************************************/
/*  WeightedTreeNode Implementation                                           */
/******************************************************************************/
/******************************************************************************/


WeightedTreeNode::WeightedTreeNode() {
    this->value = 0;
    this->weight = -1;
}

WeightedTreeNode::WeightedTreeNode(int value, 
                                   ull weight, 
                                   WeightedTreeNode* n1, 
                                   WeightedTreeNode* n2) {
    this->value = value;
    this->weight = weight;
    this->left = n1;
    this->rght = n2;
}

WeightedTreeNode::WeightedTreeNode(int value, 
                                   WeightedTreeNode* n1, 
                                   WeightedTreeNode* n2) {
    this->left = n1;
    this->rght = n2;
    this->weight = n1->getWeight() + n2->getWeight();
    this->value = value;
    
    left->incDepths();
    rght->incDepths();
}

WeightedTreeNode::WeightedTreeNode(int value, ull weight) : WeightedTreeNode() {
    this->value = value;
    this->weight = weight;
    this->depth = 0;
}

WeightedTreeNode* WeightedTreeNode::getLeft() {
    return left;
}

WeightedTreeNode* WeightedTreeNode::getRght() {
    return rght;
}

int WeightedTreeNode::getValue() {
    return value;
}

ull WeightedTreeNode::getWeight() {
    return weight;
}

int WeightedTreeNode::getDepth() const {
    return depth;
}

void WeightedTreeNode::incDepths() {
    vector<WeightedTreeNode*> dfs;
    
    dfs.push_back(this);
    
    WeightedTreeNode* current;
    
    while (! dfs.empty()){
        current = dfs.back();
        if (current->getValue() == 0){
            current->incDepth();
            dfs.pop_back();
            dfs.push_back(current->getLeft());
            dfs.push_back(current->getRght());
        } else {
            current->incDepth();
            dfs.pop_back();
        }
    }
}

void WeightedTreeNode::incDepth() {
    depth++;
}


ostream& operator << (ostream& os, const WeightedTreeNode& w){
    os << w.value;
    return os;
}

ostream& operator << (ostream& os, const WeightedTreeNode* w){
    os << w->value;
    return os;
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-WeightedTreeNode" button-text="WeightedTreeNode" toggle-text=text-capture  footer="End of WeightedTreeNode"%}

Note that the assignment didn't actually want me to encode/decode anything, it just wanted me to determine what would be the longest and shortest possible Bit codes needed.  As such my code doesn't create an encoding tree, utility-wise, but it can be used to determine the depths of the tree.  Here is the general algorithm, as presented from lecture, that was employed in my code below: 

![img]({{ '/assets/images/20180817/Algorithms-pt3-wk3-ex6.png' | relative_url }}){: .center-image }

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}

//huffman codes
void solvePart3Week3Q12(){
    //    string filename = "/home/nathan/Extracts/stanford-algs/testCases/course3/assignment3HuffmanAndMWIS/question1And2/input_random_1_10.txt";
    
    string filename = "/home/nathan/Programming/OSSU/Core_Theory/Algorithms-Roughgarden/Part3/Week3/huffman.txt";
    
    ifstream infile(filename);
    
    string line;
    
    getline(infile, line);
    
    int numberOfNodes = stoi(line);
    ull readWeight;
    int nodeValue = 1;
    pqueue<WeightedTreeNode*, WeightedTreeNodeComparator<WeightedTreeNode> > nodes;
    
    //read in value weights and create a WeightedTreeNode for each.
    //Nodes are sorted in ascending order of weight
    while (getline(infile, line)){
        readWeight = stoull(line);
        nodes.push(new WeightedTreeNode(nodeValue, readWeight)); //O(nlogn)
        nodeValue++;
    }
 
    
    WeightedTreeNode* n1;
    WeightedTreeNode* n2;
    
    WeightedTreeNode* combined;
    
    //assumes more than 1 node at start, create the encoding tree
    while (nodes.size() > 1){
        n1 = nodes.top();
        nodes.pop();
        
        n2 = nodes.top();
        nodes.pop();
        
        combined = new WeightedTreeNode(0, n1, n2);
        nodes.push(combined); //O(nlogn)
    } //end of creating encoding tree
    
    
    //now determine min and max code lengths, via depth first search
    int maxDepth = INT32_MIN;
    int minDepth = INT32_MAX;
    
    vector<WeightedTreeNode*> dfs; //depth first search, facilitated by stack
    
    dfs.push_back(nodes.top());
    WeightedTreeNode* current;
    
    while ( ! dfs.empty()){
        current = dfs.back();
        if(current->getValue() == 0 ){
            dfs.pop_back();
            dfs.push_back(current->getLeft());
            dfs.push_back(current->getRght());
        } else {
            maxDepth = max(current->getDepth(), maxDepth);
            minDepth = min(current->getDepth(), minDepth);
            dfs.pop_back();
        }
    }
    
    
    cout << maxDepth << endl << minDepth << endl;
    infile.close();
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-HuffmansAlgorithm" button-text="Huffman's Algorithm" toggle-text=text-capture  footer="End of Huffman's Algorithm"%}

Interestingly, one of the assignments in my Scala functional programming course also had me implement Huffman's algorithm, but in functional programming style.  Compare my OOP solution to my FP solution [here]({% post_url 2018-09-11-Scala-Spec-Pt-1-Wk-4 %}).
### PA 3

The third problem has us employ dynamic programming to compute the maximum-weight independent set of a path graph.  