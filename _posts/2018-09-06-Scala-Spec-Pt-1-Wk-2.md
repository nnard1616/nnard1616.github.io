---
layout: post
title: "Scala Specialization Part 1 Week 2"
description: My thoughts and answers to this week's problems.
date: 2018-09-06
comments: true
tags:
 - coursera
 - scala
---

For this week's exercises we're asked to implement a 'set' data structure for integers.  In addition, we're asked to practice writing unit tests, which is super easy and straightforward in Scala.  Below is my set implementation as well as the unit tests I developed for it:

{% capture text-capture %}
>Scala
{:.filename}
{% highlight Scala linenos %}

package funsets

/**
  * 2. Purely Functional Sets.
  */
object FunSets {

  /**
    * We represent a set by its characteristic function, i.e.
    * its `contains` predicate.
    */
  type Set = Int => Boolean

  /**
    * Indicates whether a set contains a given element.
    */
  def contains(s: Set, elem: Int): Boolean = s(elem)

  /**
    * Returns the set of the one given element.
    */
  def singletonSet(elem: Int): Set = (i: Int) => (i == elem)

  /**
    * Returns the union of the two given sets,
    * the sets of all elements that are in either `s` or `t`.
    */
  def union(s: Set, t: Set): Set = (i: Int) => (s(i) || t(i))

  /**
    * Returns the intersection of the two given sets,
    * the set of all elements that are both in `s` and `t`.
    */
  def intersect(s: Set, t: Set): Set = (i: Int) => (s(i) && t(i))

  /**
    * Returns the difference of the two given sets,
    * the set of all elements of `s` that are not in `t`.
    */
  def diff(s: Set, t: Set): Set = (i: Int) => (s(i) && !t(i))

  /**
    * Returns the subset of `s` for which `p` holds.
    */
  def filter(s: Set, p: Int => Boolean): Set = (i: Int) => (s(i) && p(i))

  /**
    * The bounds for `forall` and `exists` are +/- 1000.
    */
  val bound = 1000

  /**
    * Returns whether all bounded integers within `s` satisfy `p`.
    */
  def forall(s: Set, p: Int => Boolean): Boolean = {
    def iter(a: Int): Boolean = {
      if (a > bound) true
      else if (s(a) && !p(a)) false
      else iter(a + 1)
    }
    iter(-bound)
  }

  /**
    * Returns whether there exists a bounded integer within `s`
    * that satisfies `p`.
    */
  def exists(s: Set, p: Int => Boolean): Boolean = !forall(s, (i: Int) => !p(i))

  /**
    * Returns a set transformed by applying `f` to each element of `s`.
    */
  def map(s: Set, f: Int => Int): Set =
    (i: Int) => exists(s, (j: Int) => f(j) == i)

  /**
    * Displays the contents of a set
    */
  def toString(s: Set): String = {
    val xs = for (i <- -bound to bound if contains(s, i)) yield i
    xs.mkString("{", ",", "}")
  }

  /**
    * Prints the contents of a set on the console.
    */
  def printSet(s: Set) {
    println(toString(s))
  }
}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-setimplementation" button-text="Set Implementation" toggle-text=text-capture  footer="End of Set Implementation"%}

{% capture text-capture %}
>Scala
{:.filename}
{% highlight Scala linenos %}

package funsets

import org.scalatest.FunSuite

import org.junit.runner.RunWith
import org.scalatest.junit.JUnitRunner

/**
  * This class is a test suite for the methods in object FunSets. To run
  * the test suite, you can either:
  *  - run the "test" command in the SBT console
  *  - right-click the file in eclipse and chose "Run As" - "JUnit Test"
  */
@RunWith(classOf[JUnitRunner])
class FunSetSuite extends FunSuite {

  /**
    * Link to the scaladoc - very clear and detailed tutorial of FunSuite
    *
    * http://doc.scalatest.org/1.9.1/index.html#org.scalatest.FunSuite
    *
    * Operators
    *  - test
    *  - ignore
    *  - pending
    */
  /**
    * Tests are written using the "test" operator and the "assert" method.
    */
  // test("string take") {
  //   val message = "hello, world"
  //   assert(message.take(5) == "hello")
  // }

  /**
    * For ScalaTest tests, there exists a special equality operator "===" that
    * can be used inside "assert". If the assertion fails, the two values will
    * be printed in the error message. Otherwise, when using "==", the test
    * error message will only say "assertion failed", without showing the values.
    *
    * Try it out! Change the values so that the assertion fails, and look at the
    * error message.
    */
  // test("adding ints") {
  //   assert(1 + 2 === 3)
  // }

  import FunSets._

  test("contains is implemented") {
    assert(contains(x => true, 100))
  }

  /**
    * When writing tests, one would often like to re-use certain values for multiple
    * tests. For instance, we would like to create an Int-set and have multiple test
    * about it.
    *
    * Instead of copy-pasting the code for creating the set into every test, we can
    * store it in the test class using a val:
    *
    *   val s1 = singletonSet(1)
    *
    * However, what happens if the method "singletonSet" has a bug and crashes? Then
    * the test methods are not even executed, because creating an instance of the
    * test class fails!
    *
    * Therefore, we put the shared values into a separate trait (traits are like
    * abstract classes), and create an instance inside each test method.
    *
    */
  trait TestSets {
    val s1 = singletonSet(1)
    val s2 = singletonSet(2)
    val s3 = singletonSet(3)
  }

  /**
    * This test is currently disabled (by using "ignore") because the method
    * "singletonSet" is not yet implemented and the test would fail.
    *
    * Once you finish your implementation of "singletonSet", exchange the
    * function "ignore" by "test".
    */
  test("singletonSet(1) contains 1") {

    /**
      * We create a new instance of the "TestSets" trait, this gives us access
      * to the values "s1" to "s3".
      */
    new TestSets {

      /**
        * The string argument of "assert" is a message that is printed in case
        * the test fails. This helps identifying which assertion failed.
        */
      assert(contains(s1, 1), "Singleton")
    }
  }

  test("union contains all elements of each set") {
    new TestSets {
      val s = union(s1, s2)
      val u = union(s, s3)
      assert(contains(s, 1), "Union 1")
      assert(contains(s, 2), "Union 2")
      assert(!contains(s, 3), "Union 3")

      assert(contains(u, 1), "Union 4")
      assert(contains(u, 2), "Union 5")
      assert(contains(u, 3), "Union 6")
    }
  }

  test("intersect contains elements common between sets") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s2, s3)

      val u = intersect(s, t)
      assert(contains(u, 2), "Intersection 1")
      assert(!contains(u, 1), "Intersection 2")
      assert(!contains(u, 3), "Intersection 3")
    }
  }

  test("difference contains elements in s not in t") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s2, s3)

      val u = diff(s, t)
      assert(contains(u, 1), "Difference 1")
      assert(!contains(u, 2), "Difference 2")
      assert(!contains(u, 3), "Difference 3")
    }
  }

  test("filter contains elements in s that satisfy p") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s, s3)

      val u = filter(t, (i: Int) => i % 2 == 1)

      assert(contains(u, 1), "Filter 1")
      assert(!contains(u, 2), "Filter 2")
      assert(contains(u, 3), "Filter 3")
    }
  }

  test("forall determines whether all elements in s satisfy p") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s, s3)

      val u = forall(s, (i: Int) => i > 0)

      assert(u == true, "Forall 1")
    }
  }

  test("exists determines whether an element in s satisfies p") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s, s3)

      val u = exists(s, (i: Int) => i == 2)
      val v = exists(s, (i: Int) => i == 5)

      assert(u == true, "Exists 1")
      assert(v == false, "Exists 2")
    }
  }

  test("map transforms every element in s with some function f") {
    new TestSets {
      val s = union(s1, s2)
      val t = union(s, s3)

      val u = map(t, (i: Int) => i * 2)

      assert(contains(u, 2), "Map 1")
      assert(contains(u, 4), "Map 2")
      assert(contains(u, 6), "Map 3")

      assert(!contains(u, 1), "Map 4")
      assert(!contains(u, 3), "Map 5")

    }
  }

}

{% endhighlight %}
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-settests" button-text="Set Tests" toggle-text=text-capture  footer="End of Set Tests"%}