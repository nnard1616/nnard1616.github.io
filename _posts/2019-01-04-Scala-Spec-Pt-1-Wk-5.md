---
layout: post
title: "Scala Specialization Part 1 Week 5"
description: My thoughts and answers to this week's problems.
date: 2019-01-04
comments: true
tags:
 - coursera
 - scala
---


<!-- vim-markdown-toc Redcarpet -->

* [Instructions](#instructions)
    * [Proving Associative property of list concatenation](#proving-associative-property-of-list-concatenation)
        * [Natural vs Structural Induction](#natural-vs-structural-induction)
        * [The Proof](#the-proof)
    * [Proving Concatenation of Nil is commutative](#proving-concatenation-of-nil-is-commutative)
    * [Proving Reverse of a list is oscillatingly idempotent](#proving-reverse-of-a-list-is-oscillatingly-idempotent)
        * [Lemma 1 Proof](#lemma-1-proof)

<!-- vim-markdown-toc -->

# Instructions

This week there are no programming assignments, but there are a few optional proofs presented in last few video lectures of this week.  I will do my best to solve these proofs by induction. (structural and natural induction)

## Proving Associative property of list concatenation

### Natural vs Structural Induction

Natural induction is used to prove properties on the naturals or integers while structural induction is used to prove a property of some structure, ie a data structure such as a list.  They both require a base case and an induction step.  

For proving some property **P(xs)**{:.highlighted} for all lists **xs**{:.highlighted}, we must show:
* that **P(Nil)**{:.highlighted} is true (base case)
* for a list **xs**{:.highlighted} and some element **x**{:.highlighted}, that *if **P(xs)**{:.highlighted} holds, then **P(x::xs)**{:.highlighted} also holds.*{:.yhighlighted}

### The Proof

Let's show that for lists **xs**{:.highlighted}, **ys**{:.highlighted}, **zs**{:.highlighted} the induction hypothesis below is true:

```
(xs ++ ys) ++ zs = xs ++ (ys ++ zs)  (*)
```

Consider this implementation of concat, which has the same behavior as "++" in the concatenation of two lists:

>Scala
{:.filename}
{% highlight Scala linenos %}

def concat[T](xs: List[T], ys: List[T]) = xs match {
    case Nil => ys                         
    case x :: xs1 => x :: concat(xs1, ys) 
}

{% endhighlight %}

This yields two clauses for list concatentation:

``` 
       Nil ++ ys = ys                 // 1st clause 
(x :: xs1) ++ ys = x :: (xs1 ++ ys)   // 2nd clause
```

First we start with the base case, where xs = Nil.  For the left side of (\*) we have:

```
(Nil ++ ys) ++ zs = 
                  = ys ++ zs  // by 1st clause
```

For the right side of (\*) we have:

```
Nil ++ (ys ++ zs) = 
                  = ys ++ zs  // by 1st clause
```

Thus the base case is established.

Now for the induction step:

Since the base case holds, we assume that (\*) holds for all lists from **Nil**{:.highlighted} to **xs**{:.highlighted} and we will now show that this implies that concatentation is also associative for a list of size one more than **xs**{:.highlighted} and thereby finishing the proof for concatenation associativity.

We replace **xs**{:.highlighted} with **x :: xs**{:.highlighted} in (\*), and prove both left and right sides equate to the same expression.  First the left side:

```
((x :: xs) ++ ys) ++ zs = 
                        = (x :: (xs ++ ys)) ++ zs // by 2nd clause
                        = x :: ((xs ++ ys) ++ zs) // by 2nd clause
                        = x :: (xs ++ (ys ++ zs)) // by induction hypothesis (*)
```

And now the right side:

```
(x :: xs) ++ (ys ++ zs) = 
                        = x :: (xs ++ (ys ++ zs)) // by 2nd clause
```

As you can see, both sides were shown to be equivalent to the same expression and thus the proof is complete.**&#8718;**{: .highlighted}

## Proving Concatenation of Nil is commutative

We will show by induction on **xs**{:.highlighted} that *xs ++ Nil = xs*{:.yhighlighted}.

Base case, where **xs**{:.highlighted} is equal to **Nil**{:.highlighted}:

```
Nil ++ Nil =
           = Nil // by 1st clause
```

Induction step, where **x :: xs**{:.highlighted} is plugged in for **xs**{:.highlighted}.  First left side:

```
(x :: xs) ++ Nil = 
                 = x :: (xs ++ Nil) // by 2nd clause
                 = x :: xs          // by induction hypothesis
```

Now for the right side:

```
x :: xs = x :: xs //No steps necessary, already at target expression
```

Since both the left and right sides equate to the same expression, the hypothesis is proven. **&#8718;**{: .highlighted}


## Proving Reverse of a list is oscillatingly idempotent

Consider this relatively inefficient (easier to work with in proof) implementation of reverse:

>Scala
{:.filename}
{% highlight Scala linenos %}

def reverse[T](xs: List[T]) = xs match {
    case Nil      => xs                         
    case x :: xs1 => reverse(xs1) ++ List(x)
}

{% endhighlight %}

From this implementation we can make out the following clauses for list reversal:

```
      Nil.reverse = Nil                    // 1st clause
(x :: xs).reverse = xs.reverse ++ List(x)  // 2nd clause
```

We'd like to prove that list reversal is "osciallatingly idempotent", ie:

```
xs.reverse.reverse = xs  (**)
```

Base case, let  **xs**{:.highlighted} be **Nil**{:.highlighted}:

```
Nil.reverse.reverse = 
                    = Nil.reverse // by 1st clause
                    = Nil         // by 1st clause
```

Induction step, we assume the hypothesis holds for all lists from **Nil**{:.highlighted} to **xs**{:.highlighted}.  We now show that this implies that the hypothesis also holds for a list one more larger than **xs**{:.highlighted}, ie **x :: xs**{:.highlighted}.

```
(x :: xs).reverse.reverse = 
                          = (xs.reverse ++ List(x)).reverse   // by 2nd clause
                          = x :: xs.reverse.reverse           // by Lemma 1  
                          = x :: xs                           // by induction hypothesis
```

And thus the hypothesis is proven for all lists. **&#8718;**{: .highlighted}

### Lemma 1 Proof



