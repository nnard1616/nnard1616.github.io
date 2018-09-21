---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 1, Week 4"
description: My thoughts and solutions for this week's exercises.
date: 2018-06-30
comments: true
tags:
 - coursera
 - roughgarden
 - algorithms
 - java
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

*Note 2: Most of this course is a carbon copy of Algorithms Illuminated Pt 1.  See my previous posts on Algorithms Illuminated for content covered in the earlier weeks of this course.*{: .highlighted}

## Quiz

### P1

In a tree of n nodes and n-1 edges, a minimum cut can be made across every edge.  Therefore there are **n-1**{:.highlighted}  minimum cuts in a tree. 

### P2

All statements that are in agreement to "For every graph *G* with *n* nodes and every min cut (**A**,**B**) of G, <img src="http://latex.codecogs.com/svg.latex?\color{Orange}Pr[out = (A,B)] \geq p" class="latexinline"/>" are correct.
### P3

This is the same as exercise 6.1 from Algorithms Illuminated Pt 1.  Below is my copy/pasted answer from my previous post:

In order for the first recursive call to be passed a subarray of length at most **&alpha;n**{:.highlighted}, where **&alpha;**{:.highlighted} is strictly between 0.5 and 1, the pivot must be located between two indices at **&alpha;n**{:.highlighted} and **n - &alpha;n**{:.highlighted} from one endpoint of the array.  The size of this region is **&alpha;n - (n - &alpha;n) = n(2&alpha; - 1)**{:.highlighted}.  Thus **2&alpha; - 1**{:.highlighted} is the fraction of the array that the pivot can be in and is therefore the probability of the conditions being satisfied. 

### P4

This is the same as exercise 6.2 from Algorithms Illuminated Pt 1.  Below is my copy/pasted answer from my previous post:

This will require the same reasoning used in problem [5.3]({% post_url 2018-06-23-Algorithms-Illuminated-Chapter-5-Exercises %}).  The maximum number of successive recursive calls occurs in the worst case scenario, which is when the subarray is **&alpha;n**{:.highlighted}.  Hence, the answer will be <img src="http://latex.codecogs.com/svg.latex?\color{Orange}log_{\alpha^-1}(n)" class="latexinline"/>.  

### *P5

The minimum s-t cut problem is the following. The input is an undirected graph, and two distinct vertices of the graph are labelled "s" and "t". The goal is to compute the minimum cut (i.e., fewest number of crossing edges) that satisfies the property that s and t are on different sides of the cut.

Suppose someone gives you a subroutine for this s-t minimum cut problem via an API. Your job is to solve the original minimum cut problem (the one discussed in the lectures), when all you can do is invoke the given min s-t cut subroutine. (That is, the goal is to reduce the min cut problem to the min s-t cut problem.)

Now suppose you are given an instance of the minimum cut problem -- that is, you are given an undirected graph (with no specially labelled vertices) and need to compute the minimum cut. What is the minimum number of times that you need to call the given min s-t cut subroutine to guarantee that you'll find a min cut of the given graph?


For a given node, s, there are n - 1 other nodes that could be t.  Each s-t pair has one min cut.  So to be sure that a min cut between s-t  is found, n - 1 calls to the min s-t cut subroutine are required.

## Programming Assignment

We're asked to find the min cut of a provided undirected graph using the proposed random algorithm.  Below is my implementation, though it runs longer than I would like.  The videos mentioned to do n^2logn repetitions in order to have a virtual guaranteed correct answer (99.6% chance success by my calculations).  However, when I monitor the process it's able to come up with the correct answer in much fewer repetitions.  With n = 200, the required repetitions should be somewhere between 100,000 to 200,000.  But most times it gets the correct answer within 100 reps.  With 100,000 reps my implementation completes in about 2.5 minutes.  Below is my implementation.  Two ways to improve it would be:
1. Java makes it really difficult to copy/clone objects, so currently for programming convenience I just have it re-read in the data when I want to start a new repetition.  This is costly I believe and if I took the time to properly implement copying of the graph object it could probably run faster.
2. There's a nested for loop in the contract method.  One way to eliminate this nested loop would be to define Node and Edge data structures and just randomly select edges to contract.  Updating the Graph would not need this nested for loop then.

>Java
{:.filename}
{% highlight java linenos %}
public class Graph implements Cloneable {
    
    /**************************************************************************/
    /*  Attributes                                                            */
    /**************************************************************************/
    private HashMap<Integer,ArrayList<Integer>> adjList;
    private int minCut = Integer.MAX_VALUE;
    private String filename;
    private static Random rand = new Random();
    
    
    /**************************************************************************/
    /*  Constructor                                                           */
    /**************************************************************************/
    public Graph(String filename){
        adjList = new HashMap<>();
        this.filename = filename;
    }
    
    
    /**************************************************************************/
    /*  Graph Methods                                                         */
    /**************************************************************************/
    private void addNode(Integer n, ArrayList<Integer> adjs){
        adjList.put(n, adjs);
    }
    
    private int cutDegree(){
        if (adjList.size() != 2)
            throw new IllegalStateException("Cut has not been made yet.");
        
        return adjList.get((Integer)adjList.keySet().toArray()[0]).size();
    }
    
    private void contract( Integer a, Integer b){
        Iterator itrA = adjList.get(a).iterator();
        Iterator itrB = adjList.get(b).iterator();
        
        //remove a from b's neighbors (adjacents)
        while (itrA.hasNext())
            if (itrA.next().equals(b))
                itrA.remove();
        
        //remove b from a's neighbors
        while (itrB.hasNext())
            if (itrB.next().equals(a))
                itrB.remove();
        
        //Determine larger and smaller values
        Integer minimum = Math.min(a, b);
        Integer maximum = Math.max(a, b);
        
        //cycle through adjs of larger valued node... 
        ArrayList<Integer> adjMax = adjList.get(maximum); //adjs of max
        ArrayList<Integer> adjMin = adjList.get(minimum); //adjs of min
        ArrayList<Integer> adjI;
        
        /* Point where improvements can be made.*/
        
        for (int i = 0; i < adjMax.size(); i++){
            adjI = adjList.get(adjMax.get(i));
            
            //cycle through adjs of adjs of larger valued node...
            for (int j = 0; j < adjI.size(); j++){
                
                //If see larger value in adjs of adjs, convert to smaller value.
                if (adjI.get(j).equals(maximum))
                    adjI.set(j, minimum);
            }
            
            //put adjs of larger into smaller
            adjMin.add(adjMax.get(i));
        }
        
        //delete larger value
        adjList.remove(maximum);
        
    }
    
    private void readInData(String filename){
        ArrayList<Integer> adjs;
        Integer n;
        Scanner lineScanner;
        Scanner intScanner;
        
        adjList.clear();
        
        try{
            lineScanner = new Scanner(new File(filename));
            
            while (lineScanner.hasNextLine()){
                
                
                adjs       = new ArrayList<>();
                intScanner = new Scanner(lineScanner.nextLine());
                n          = intScanner.nextInt();
                
                while (intScanner.hasNextInt())
                    adjs.add(intScanner.nextInt());
                
                addNode(n, adjs);
            }
        }catch(FileNotFoundException fnfe){
            System.out.println("no file found");
        }
    }
    
    public int determineMinCut(){
        readInData(filename);
        int reps = 1000;

        for (int i = 0; i < reps; i++){


            while (getAdjList().size() > 2){
                //randomly select node
                Integer randFirst = rand.nextInt(adjList.size());
                Integer a = (Integer)adjList.keySet().toArray()[randFirst];

                //randomly select neighbor of first node
                Integer randSecond = rand.nextInt(adjList.get(a).size());
                Integer b = adjList.get(a).get(randSecond);

                contract(a, b);
            }

            minCut = Math.min(minCut, cutDegree());

            //reset
            readInData(filename);
        }

        return minCut;
    }

    
    /**************************************************************************/
    /*  Getters                                                               */
    /**************************************************************************/
    public HashMap<Integer, ArrayList<Integer>> getAdjList() {
        return adjList;
    }

    public int size(){
        return adjList.keySet().size();
    }
    
    
    /**************************************************************************/
    /*  Overrides                                                             */
    /**************************************************************************/
    @Override
    public String toString() {
        String result = "";
        
        for (Integer i: adjList.keySet()){
            result += i + " ";
            for (Integer j : adjList.get(i)){
                result += j + " ";
            }
            result += "\n";
        }
        
        return result;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return new Graph(filename);
    }
}
{% endhighlight %}
