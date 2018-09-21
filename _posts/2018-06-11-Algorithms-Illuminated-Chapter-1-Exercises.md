---
layout: post
title: "Algorithms Illuminated Pt 1, Ch 1 Exercises"
description: My thoughts and solutions for this chapter's exercises.
date: 2018-06-11
comments: true
tags:
 - algorithmsilluminated
 - roughgarden
 - bookexercises
 - java
--- 

*Note: those marked with an asterisk I am not 100% confident in my answer/reasoning.*{: .highlighted}

*Please feel free to discuss the problems and my proposed solutions below!*{: .highlighted}

### 1.1
Assuming 0 based counting, 4 will be in position 7.

### 1.2
I would think it should be <img src="http://latex.codecogs.com/svg.latex?\color{Orange}O(n\cdot log_3(n))"/>, but that's not an answer choice.  Additionally that would be an improvement of the 2-way merge sort, and I'm not so sure that 3-way mergesort should be better.  I would think it should perform worse as there are now more comparisons being made at the merge steps.  Indeed, if you were to keep making more and more divisions, say make `n` groups as opposed to 2 or 3 groups, then you've obtained a virtual selection sort algorithm.  And selection sort runs at <img src="http://latex.codecogs.com/svg.latex?\color{Orange}O(n^2)"/>.  

My guess is going to be b, <img src="http://latex.codecogs.com/svg.latex?\color{Orange}O(n\cdot log(n))"/>.

### 1.3
Taking two sorted arrays of length `n`, there should be at most `n` comparisons. If you take that `2n` item array and merge with the third `n` array, then in the worst case `2n` comparisons will be made (first half of the `2n` array are lesser than the incoming elements), giving a total of `3n` comparisons.  One would be tempted to stop there and say it should be `nk`, as 3 was obtained after merging 3 `n` arrays.  But the number of comparisons grows by the triangular numbers: `1,3,6,10,...` which I believe indicates some sort of squaring on k is occurring. 

So my guess is e, <img src="http://latex.codecogs.com/svg.latex?\color{Orange}nk^2" class="latexinline"/>.

### 1.4
Now that we're applying more "divide and conquer", we should have a log factor now.  I think it should be `logk` as we're dividing the number of arrays each time.  

My guess is c, <img src="http://latex.codecogs.com/svg.latex?\color{Orange}nklog(k)" class="latexinline"/>.

### 1.5
This problem is pretty similar to the classic problem of:  
>If we arrange a single elimination tournament with `n` contestants, how many matches will need to be organized?  

The intuition I've used for that classic problem is: in order for there to be 1 winner, there must be (*)`n - 1` losers.  In order to have a loser, you have to play a match.  Since there's `n - 1` losers, then that's how many matches are needed to determine a winner.   

The same principle can be performed on this unsorted array, similar to merge sort.  But for the second largest value (SLV) we'll need to do some extra steps.  Particularly, we'll need to keep a record of each defeated opponent for each value in our list.  Once we determine what the largest value (LV) is, the SLV will be one of the defeated opponents that the LV faced against in the tournament.  The number of defeated opponents amounts to the number of rounds in the tournament.  Since the number of values is a power of 2, there will be `log_2(n)` rounds in the tournament.  Since that's the number of rounds, then that's the number of opponents the LV will face.  So to determine the SLV, have a mini tournament of these `log_2(n)` values.  Using the same principal from (*), there must be `log_2(n) - 1` matches to determine the SLV.  So then the total number of comparisons amounts to: <img src="http://latex.codecogs.com/svg.latex?\color{Orange}(n-1)+(log_2(n)-1) = n+log_2(n)-2" class="latexinline"/>.**∎**{: .highlighted}

---

## Programming Challenge
### 1.6

This one took me longer than I think it should have.  It's super important to plan things out before hand, which I did not do.  I will try to do more preliminary planning in future projects.  

There's probably some simplifying I could do, such as eliminate some steps in which I trim leading zeroes.  Either way, I've designed a successful project which computes the correct answer in only a few milliseconds.

>Java
{:.filename}
{% highlight java %}
public class XLong {
    
    private String number;
    
    /**************************************************************************/
    /*  Constructors                                                          */
    /**************************************************************************/
    public XLong() {
        this("0");
    }
    
    public XLong(String number){
        if (isValidNumber(number))
            this.number = number;
        else
            throw new IllegalStateException(number + " is not an integer.");
    }
    
    /**************************************************************************/
    /*  Checkers                                                              */
    /**************************************************************************/
    private boolean isValidNumber(String candidate){
        String validNums = "0123456789";
        
        //check that each digit is one of the valid numbers.
        for (char c : candidate.toCharArray())
            if (!validNums.contains(Character.toString(c)))
                return false;
        
        return true;
    }
    
    private boolean isPowerOf2(int n){
        if (n == 1)
            return true;
        
        if (n%2 == 0)
            n /= 2;
        else
            return false;
        
        return isPowerOf2(n);
    }
    
    
    /**************************************************************************/
    /*  Getters & Setter                                                      */
    /**************************************************************************/
    public int length(){
        return this.number.length();
    }
    
    public String getNumber() {
        return number;
    }
    
    public void setNumber(String number) {
        this.number = number;
    }

    
    /**************************************************************************/
    /*  Helpers                                                               */
    /**************************************************************************/
    /**
     * Adds leading zeroes to other number until both this and other
     * are the same length.
     * 
     * @param other
     */
    private void tackOnLeadingZeroesTo(XLong other){
        while(other.length() < this.length())
            other.setNumber("0" + other.getNumber());
    }
    
    /**
     * Adds n trailing zeroes to this number.
     * @param other 
     */
    private void tackOnTrailingZeroes(int n){
        while (n > 0){
            this.setNumber(this.getNumber() + "0");
            n--;
        }
    }
    
    private void trimLeadingZeroes(){
        if (length() == 1)
            return;
        
        // need to also check that length is > 1 so that numbers like '00' 
        // don't cause errors.
        while (this.getNumber().charAt(0) == '0' && length() > 1)
            this.setNumber(String.copyValueOf(this.getNumber().toCharArray(), 
                                              1, 
                                              this.length()-1));
    }
    
    /**
     * Converts chars '0','1','2','3','4','5','6','7','8','9' to their literal
     * int variants.
     * 
     * @param digit
     * @return int literal of digit
     */
    private int string2Int(String number){
        int result = 0;
        int zeroes = number.length()-1; // determines place value of digit.
        
        for (char c : number.toCharArray()){
            result += (c - '0')*Math.pow(10,zeroes);
            zeroes--;
        }
        return result;
    }
    
    
    /**************************************************************************/
    /*  Math Operations                                                       */
    /**************************************************************************/
    /**
     * Assumes both XLongs have lengths that are powers of 2 and are the same
     * length.
     * 
     * @param other
     * @return product of this and other XLong.
     */
    public XLong karatMultiplyBy(XLong other){
        
        //base case, both numbers are single digit values.
        if (length() == 1 && other.length() == 1){
            int n1 = string2Int(number);
            int n2 = string2Int(other.getNumber());
            int p = n1*n2;
            return new XLong(""+p);
        }
        
        //determine which number is the longer and shorter number.
        XLong longer;
        XLong shorter;
        if (length() > other.length()){
            longer = this;
            shorter = other;
        }else{
            longer = other;
            shorter = this;
        }
        
        //add leading zeroes to the longer number until its length is a power
        //of 2.
        while(!isPowerOf2(longer.length()))
            longer.setNumber("0" + longer.getNumber());
        
        //add leading zeroes to the shorter number until it matches the length
        //of the longer number.
        longer.tackOnLeadingZeroesTo(shorter);
        
        XLong a = new XLong(String.copyValueOf(number.toCharArray(), 
                                               0, 
                                               length()/2));
        
        XLong b = new XLong(String.copyValueOf(number.toCharArray(), 
                                               length()/2, 
                                               length()/2));
        
        XLong c = new XLong(String.copyValueOf(other.getNumber().toCharArray(), 
                                               0, 
                                               other.length()/2));
        
        XLong d = new XLong(String.copyValueOf(other.getNumber().toCharArray(), 
                                               other.length()/2, 
                                               other.length()/2));
        
        
        XLong ac   = a.karatMultiplyBy(c);
        XLong bd   = b.karatMultiplyBy(d);
        XLong foil = (a.add(b)).karatMultiplyBy(c.add(d));//(a+b)(c+d)
        XLong sub  = ac.add(bd);
        XLong diff = foil.subtract(sub);//(a+b)(c+d)-ac-bd
        
        
        //multiply ac by 10^n, if ac is not 0
        if (!ac.getNumber().equals("0"))
            ac.tackOnTrailingZeroes(length());
        
        //multiply ((a+b)(c+d)-ac-bd) by 10^n/2
        diff.tackOnTrailingZeroes(length()/2);
        
        //bring it all together to make the answer.
        XLong result = (ac.add(diff)).add(bd);
        
        return result;
    }
    
    /**
     * Adds the numbers together.
     * @param other
     * @return sum of this and other XLong.
     */
    public XLong add(XLong other){
        String sum = "";
        int carry = 0; //for when we need to carry a 1.  0 otherwise.
        
        //make sure both numbers are of the same length, add leading zeroes
        //to the shorter number if not.
        if (this.length() > other.length())
            this.tackOnLeadingZeroesTo(other);
        if (other.length() > this.length())
            other.tackOnLeadingZeroesTo(this);
        
        //cycle through digits, starting with ones place (last index), adding 
        //the numbers and a carry if one was generated in the previous cycle.
        for (int i = length()-1; i >= 0; i--){
            
            //convert chars to ints
            int d1 = number.charAt(i) - '0';
            int d2 = other.getNumber().charAt(i) -'0';
            
            int s = d1 + d2 + carry;
            
            //check if we need to carry a 1
            if (s >= 10){
                carry = 1;
                s -= 10;
            }else
                carry = 0;
            
            sum = s + sum;
        }
        
        if (carry == 1)
            sum = 1 + sum;
        
        //get rid of leading zeroes.
        this.trimLeadingZeroes();
        other.trimLeadingZeroes();
        
        
        return new XLong(sum);
    }
    
    /** Assumes number to be subtracted is smaller than the current number.
     * 
     * @param other
     * @return difference of other number from this number.
     */
    public XLong subtract(XLong other){
        XLong otherCopy = new XLong(other.toString());
        XLong step = new XLong("1");
        XLong accumulator = new XLong(); //keeps track of difference
        
        this.tackOnLeadingZeroesTo(otherCopy);
        
        //add steps to otherCopy and accumulator until otherCopy equals this 
        //number.  Step size increases by factor of 10 each time a place value
        //is matched with this number.
        
        //cycle through digits, starting with ones place.
        for(int i = length()-1; i >= 0; i--){
            
            //keep adding step to otherCopy until current place value
            //being examined matches that of this number's corresponding 
            //place value.
            while(otherCopy.getNumber().charAt(i) != number.charAt(i)){
                otherCopy = otherCopy.add(step);
                accumulator = accumulator.add(step);
            }
            step.setNumber(step.getNumber()+ "0");//multiply stepsize by 10
        }
        
        return accumulator;
    }

    
    /**************************************************************************/
    /*  Overrides                                                             */
    /**************************************************************************/
    @Override
    public String toString() {
        return this.number;
    }

    @Override
    public boolean equals(Object obj) {
        return number.equals(((XLong)obj).getNumber());
    }
}
{% endhighlight %}

This computes the answer to be:

8 539 734 222 673 567 065 463 550 869 546 574 495 034 888 535 765 114 961 879 601 127 067 743 044 893 204 848 617 875 072 216 249 073 013 374 895 871 952 806 582 723 184

which <a href="http://www.wolframalpha.com/input/?i=3141592653589793238462643383279502884197169399375105820974944592*2718281828459045235360287471352662497757247093699959574966967627">wolframalpha</a> affirms.
