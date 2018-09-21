---
layout: post
title: "Scala Specialization Part 1 Week 1"
description: My thoughts and answers to this week's problems.
date: 2018-09-05
comments: true
tags:
 - coursera
 - scala
---

![img]({{ '/assets/images/20180905/Scala-wk1-img-1.png' | relative_url }}){: .center-image }
The first exercise was pretty simple.   Here is my solution:

>Scala
{:.filename}
{% highlight Scala linenos %}
def pascal(c: Int, r: Int): Int = {
  def factorial(n: Int): Int =
    if (n == 0) 1 else n * factorial(n - 1)

  factorial(r) / (factorial(r - c) * factorial(c))
}
{% endhighlight %}

![img]({{ '/assets/images/20180905/Scala-wk1-img-2.png' | relative_url }}){: .center-image }
The second exercise was a little more challenging, but I was able to get it eventually, here is my solution:

>Scala
{:.filename}
{% highlight Scala linenos %}
def balance(chars: List[Char]): Boolean = {

  def count(acc: Int, chars: List[Char]): Int =
    if (chars.isEmpty || acc < 0) acc
    else if (chars.head == '(') count(acc + 1, chars.tail)
    else if (chars.head == ')') count(acc - 1, chars.tail)
    else count(acc, chars.tail)

  count(0, chars) == 0
}
{% endhighlight %}

![img]({{ '/assets/images/20180905/Scala-wk1-img-3.png' | relative_url }}){: .center-image }
This was very challenging, so much so I was not able to come up with a functional solution.  After some research I was able to come up with an imperative solution:

>Scala
{:.filename}
{% highlight Scala linenos %}

def countChange(money: Int, coins: List[Int]): Int = {

  val c = new Array[Int](money + 1)

  c(0) = 1

  for (k <- 1 to money) {
    c(k) = 0
  }

  for (k <- coins) {
    for (i <- 0 to money - k) {
      c(i + k) += c(i)
    }
  }

  c(money)
}
{% endhighlight %}