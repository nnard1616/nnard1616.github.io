---
layout: post
title: "Karatsuba Multiplication"
date:  2018-06-10 13:12:49 
description: My thoughts and first impressions on Karatsuba Multiplication.
tags:
 - helloworld
 - algorithmsilluminated
 - adcssra
 - roughgarden
 - coursera
--- 


## Algorithms: Divide and Conquer, Sorting and Searching, and Randomized Algorithms
<hr>
I am starting the 4 course sequence on Algorithms provided by Stanford University via Coursera, taught and organized by Tim Roughgarden.  The first course has a book that accompanies it, Algorithms Illuminated Part 1, which I will be reading through as I complete the course.  In this post I will comment on <span style="color: #C38FD6">Karatsuba Multiplication</span>, an alternative approach to elementary/manual multiplication of two numbers of n digits.
<br>

## Karatsuba Multiplication
<hr>
In Algorithms Illuminated, page 7, the method is presented as follows:

![img]({{ '/assets/images/20180610/karatsuba.jpg' | relative_url }}){: .center-image }

<span style="color: #C38FD6">*Note: the answer listed has a typo, it should be 7006652 instead.*</span>
<br>

Originally I thought it was silly, why would you subtract `ac` & `bd` from `(a+b)*(c+d)` ?  I had done some algebraic analysis:


![img]({{ '/assets/images/20180610/proof1.svg' | relative_url }}){: .latexequation }

which seemed to suggest to me that subtracting `ac` & `bd` was trivial as they would come back in the end.  I misunderstood though, as the discussion in the book was on recursive calls.  The intent of the book is to use recursion for each multiplication.  If one uses the 5 step process above you could reduce the number of recursive calls to 3: `ac`, `bd`, `(a+b)*(c+d)`; as opposed to 4 recursive calls: `ac`, `ad`, `bc`, `bd` if one were to use formula `(1)`.  Reducing the number of recursive calls seems like it would improve performance, but I will likely better understand as I read more through this book.

As it turns out, exercise 1.6 asks for an implementation of the Karatsuba Multiplication method, you can find my Java implementation [here]({% post_url 2018-06-11-Algorithms-Illuminated-Chapter-1-Exercises %}).