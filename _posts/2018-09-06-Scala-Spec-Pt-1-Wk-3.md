---
layout: post
title: "Scala Specialization Part 1 Week 3"
description: My thoughts and answers to this week's problems.
date: 2018-09-09
comments: true
tags:
 - coursera
 - scala
---

Last week's implementation of a set was more functional in style.  This week we do the same thing but more object-oriented in style.  Additionally, we work with a modern application of Scala by interacting with Tweet data from Twitter.  There's many different classes in this week's project, but there's two classes in which most of the implemenation is contained within.  They are listed below:

{% capture text-capture %}
>Scala
{:.filename}
{% highlight Scala linenos %}

class Empty extends TweetSet {

  def filterAcc(p: Tweet => Boolean, acc: TweetSet): TweetSet = acc

  def union(that: TweetSet): TweetSet = that

  def mostRetweeted: Tweet = throw new NoSuchElementException

  def descendingByRetweet: TweetList = Nil

  def isEmpty: Boolean = true

  /**
    * The following methods are already implemented
    */
  def contains(tweet: Tweet): Boolean = false

  def incl(tweet: Tweet): TweetSet = new NonEmpty(tweet, new Empty, new Empty)

  def remove(tweet: Tweet): TweetSet = this

  def foreach(f: Tweet => Unit): Unit = ()
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-Empty" button-text="Empty Set" toggle-text=text-capture  footer="End of Empty Set"%}

{% capture text-capture %}
>Scala
{:.filename}
{% highlight Scala linenos %}

class NonEmpty(elem: Tweet, left: TweetSet, right: TweetSet) extends TweetSet {

  def filterAcc(p: Tweet => Boolean, acc: TweetSet): TweetSet =
    if (p(elem)) left.filterAcc(p, acc incl elem ) union right.filterAcc(p, acc incl elem )
    else left.filterAcc(p, acc) union right.filterAcc(p, acc)

  //broken implementation....
//  def union(that: TweetSet): TweetSet =
//    if (!that.contains(elem)) (this.left union (that.incl(elem) union this.right))
//    else this.left union (that union this.right)

  //prevents duplicates like sets should
  def union(that: TweetSet): TweetSet =
    if (!that.contains(elem)) this.left union (this.right union that.incl(elem))
    else this.left union (this.right union that)

  def mostRetweeted: Tweet =
    if (!right.isEmpty && elem.retweets < right.mostRetweeted.retweets)
      right.mostRetweeted
    else if (!left.isEmpty && elem.retweets < left.mostRetweeted.retweets)
      left.mostRetweeted
    else elem

  def descendingByRetweet: TweetList =
    new Cons(mostRetweeted, remove(mostRetweeted).descendingByRetweet)

  def isEmpty(): Boolean = false

  /**
    * The following methods are already implemented
    */
  def contains(x: Tweet): Boolean =
    if (x.text < elem.text) left.contains(x)
    else if (elem.text < x.text) right.contains(x)
    else true

  def incl(x: Tweet): TweetSet = {
    if (x.text < elem.text) new NonEmpty(elem, left.incl(x), right)
    else if (elem.text < x.text) new NonEmpty(elem, left, right.incl(x))
    else this
  }

  def remove(tw: Tweet): TweetSet =
    if (tw.text < elem.text) new NonEmpty(elem, left.remove(tw), right)
    else if (elem.text < tw.text) new NonEmpty(elem, left, right.remove(tw))
    else left.union(right)

  def foreach(f: Tweet => Unit): Unit = {
    f(elem)
    left.foreach(f)
    right.foreach(f)
  }
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-NonEmpty" button-text="NonEmpty Set" toggle-text=text-capture  footer="End of NonEmpty Set"%}

Of particular interest is the union implementation in the NonEmpty set.  The order in which you apply the operations can either result in a successful program or an unsuccessful (does not terminate) program.  Briefly, the issue arrises from the fact that the highest level union call only terminates when the "left" and "right" portions become empty.  This will only happen when the "left" or "right" portions appear on the left of a union call.  Otherwise, they never become empty and the recursion never terminates.  A student, Akhmed U, had a very thorough explanation on the Coursera [forums](https://www.coursera.org/learn/progfun1/discussions/weeks/3/threads/SkerhCXDEeaRtQpho9OEXw/replies/Gl3HSpQbEeaxvRLoQ7NHzw).