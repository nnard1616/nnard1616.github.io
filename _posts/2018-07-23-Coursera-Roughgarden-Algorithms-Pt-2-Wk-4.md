---
layout: post
title: "Coursera Algoirthms with Roughgarden Pt 2, Week 4"
description: My thoughts and solutions for this week's exercises.
date: 2018-07-23
comments: true
tags:
 - coursera
 - roughgarden
 - algorithms
 - c++
 - java
--- 


*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

## Quiz

### P1
>Which of the following is not a property that you expect a well-designed hash function to have?

Hash functions are not able to spread out all data sets, the best can only spread out most data sets.

### P2
>Suppose we use a hash function *h* to hash *n* distinct keys into an array *T* of length *m*. Assuming simple uniform hashing --- that is, with each key mapped independently and uniformly to a random bucket --- what is the expected number of keys that get mapped to the first bucket? More precisely, what is the expected cardinality of the set {k:h(k)=1}?

Using Linearity of Expectation, we see that the probability of an item being hashed to a particular bucket is 1/m.  And since there's n objects, we can expect n*(1/m) = n/m objects get hashed to this particular bucket.

### P3
>Suppose we use a hash function *h* to hash *n* distinct keys into an array *T* of length *m*. Say that two distinct keys *x,y* collide under *h* if *h(x)=h(y)*. Assuming simple uniform hashing --- that is, with each key mapped independently and uniformly to a random bucket --- what is the probability that a given pair *x,y* of distinct keys collide?

This is like saying: "what is the probability two objects get placed into the same bucket?"  Using rules/paradigms of probability, we first pick any bucket for our first object (probability = 1), and then the probability of the second object landing in the same bucket will be 1/m (due to uniformity).  Multiplying together, we get 1*(1/m) = 1/m.

### P4
>Suppose we use a hash function *h* to hash *n* distinct keys into an array *T* of length mm. Assuming simple uniform hashing --- that is, with each key mapped independently and uniformly to a random bucket --- what is the expected number of pairs of distinct keys that collide? (As above, distinct keys *x,y* are said to collide if *h(x)=h(y)*.)

Similar to the last problem, using Linearity of Expectation, note that there's n C 2 pairs and the probability of any pair collides is 1/m (from last problem).  Taking the product, we get (n C 2)*(1/m) = (n (n-1)) / (2m).

### P5
>To interpret our heuristic analysis of bloom filters in lecture, we considered the case where we were willing to use 8 bits of space per object in the bloom filter. Suppose we were willing to use twice as much space (16 bits per object). What can you say about the corresponding false positive rate, according to our heuristic analysis (assuming that the number *k* of hash tables is set optimally)? [Choose the strongest true statement.]

Using the formula from lecture, 0.5^(ln(2)*16) = 0.046%.

---
## Programming Assignment
>The goal of this problem is to implement a variant of the 2-SUM algorithm covered in this week's lectures.
>
>The file contains 1 million integers, both positive and negative (there might be some repetitions!).This is your array of integers, with the i^{th} row of the file specifying the i^{th} entry of the array.
>
>Your task is to compute the number of target values tt in the interval [-10000,10000] (inclusive) such that there are distinct numbers *x,y* in the input file that satisfy *x+y=t*. (NOTE: ensuring distinctness requires a one-line addition to the algorithm from lecture.)
>
>Write your numeric answer (an integer between 0 and 20001) in the space provided.

I was able to come up with two solutions, one using and one not using a hash table.  Using a hash table is **O(tn)**{:.highlighted}, so it actually turns out to be pretty slow.  I made a java implementation which runs in about 4 and a half minutes.

{% capture text-capture %}
>Java
{:.filename}
{% highlight Java linenos %}
public class TwoSum {
    public static void solve() throws FileNotFoundException{
        Scanner sc = new Scanner(new File("/home/nathan/Programming/OSSU/"
                + "Core_Theory/Algorithms-Roughgarden/Part2/Week4/"
                + "algo1-programming_prob-2sum.txt"));
        
        Long l;
        Long complement;
        Long t;
        Integer repeats;
        int result = 0; 
        
        HashSet<Long> s = new HashSet<>();
        HashMap<Long,Integer> n = new HashMap<>(); //unassigned returns null
        
        //create range of -10000 to 10000
        for (long i = -10000; i <= 10000; i++)
            s.add(i);
        
        //read in the million numbers
        while(sc.hasNextLong()){
            l = sc.nextLong();
            repeats = n.putIfAbsent(l, 1);
            if (repeats != null)
                n.put(l, repeats+1);
        }
        
        
        Iterator itr = s.iterator();
        
        //iterate through million numbers
        for (Long number : n.keySet()){
            
            //iterate through what's left of -10000 to 10000
            while (itr.hasNext()){
                
                t = (Long)itr.next();
                complement = t - number;
                
                //check they are distinct
                if (complement != number){
                    
                    //check if both in our list of million numbers
                    if (n.containsKey(complement)){
                        
                        //remove current t from our range of -10000 to 10000,
                        //so we don't examine it again.
                        itr.remove();
                        
                        result++;
                    }
                }
            }
            
            //reset iterator
            itr = s.iterator();
        }
        
        System.out.println("Answer: " + result + "\n");
    }
}
{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-twosum" button-text="TwoSum: Hash Table" toggle-text=text-capture  footer="End of TwoSum"%}

The method that doesn't use a hash table is a lot faster, running at **O(n)**{:.highlighted}.  The method involves a "resolving, sliding window".  First sort the numbers, then create two pointers to either end of the list, and then progress the pointers to each other until you get sums that are in the -10000 to 10000 range.  Below is my c++ implementation of it, which runs at about 300 ms.  

{% capture text-capture %}
>C++
{:.filename}
{% highlight C++ linenos %}
void twoSum(){
    vector<long long> numbers;
    set<long long> results;
    
    ifstream infile("/home/nathan/Programming/OSSU/Core_Theory/"
    "Algorithms-Roughgarden/Part2/Week4/algo1-programming_prob-2sum.txt");
    
    string line;
    
    
    while (getline(infile, line)){
        numbers.push_back(stoll(line));
    }
    
    std::sort(numbers.begin(), numbers.end());
    
    auto i = numbers.begin();
    auto j = --numbers.end();
    
    while (*i <= *j){
        if (*i + *j > 10000)
            j--;
        else if (*i + *j < -10000)
            i++;
        else{
            while(*i <= *j){
                
                if (*i + *j > 10000)
                    break;
                results.insert(*i + *j);
                i++;
            }
        }
    }
    
    cout << results.size() << endl;
    
}
{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-twosum2" button-text="TwoSum: No Hash table" toggle-text=text-capture  footer="End of TwoSum"%}