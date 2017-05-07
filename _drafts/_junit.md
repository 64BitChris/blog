---
layout: post
title: "Single Statement Unit Tests"
date: 2017-05-13
place: Odessa, Ukraine
tags: testing
description: |
  Test methods are traditionally very procedural
  and algorithmic, which makes them difficult to read
  and maintain; this principle may improve them.
keywords:
  - testing
  - junit
  - hamcrest
  - junit design
  - testing design patterns
image: /images/2017/05/bullet.jpg
jb_picture:
  caption: Bullet (1996) by Julien Temple
---

There are tons of articles and books about unit testing patterns
and anti-patterns. I want to add one more recommendation, which, I believe,
can help us make tests _and_ production code more _object-oriented_:
a test method may contain nothing but a single `assert`.

<!--more-->

{% jb_picture_body %}

Look at this test method from `RandomStreamTest` from OpenJDK&nbsp;8,
created by Brian Goetz:

{% highlight java %}
@Test
public void testIntStream() {
  final long seed = System.currentTimeMillis();
  final Random r1 = new Random(seed);
  final int[] a = new int[SIZE];
  for (int i=0; i < SIZE; i++) {
    a[i] = r1.nextInt();
  }
  final Random r2 = new Random(seed);
  final int[] b = r2.ints().limit(SIZE).toArray();
  assertEquals(a, b);
}
{% endhighlight %}

There are two parts in this method: the algorithm and the assertion. The
algorithm prepares two arrays of integers and the assertion compares them
and throws `AssertionError` if they are not equal.

I'm saying that the first part, the algorithm, is the one we should try
to avoid. The only thing we must have is the assertion. Here is
how I would re-design this test method:

{% highlight java %}
@Test
public void testIntStream() {
  final long seed = System.currentTimeMillis();
  assertEquals(
    new ArrayFromRandom(
      new Random(seed)
    ).toArray(SIZE),
    new Random(seed).ints().limit(SIZE).toArray()
  );
}
private static class ArrayFromRandom {
  private final Random random;
  ArrayFromRandom(Random r) {
    this.random = r;
  }
  int[] toArray(int s) {
    final int[] a = new int[s];
    for (int i=0; i < s; i++) {
      a[i] = this.random.nextInt();
    }
    return a;
  }
}
{% endhighlight %}

If Java would have "monikers", this code would look even more elegant:

{% highlight java %}
@Test
public void testIntStream() {
  assertEquals(
    new ArrayFromRandom(
      new Random(System.currentTimeMillis() as seed)
    ).toArray(SIZE),
    new Random(seed).ints().limit(SIZE).toArray()
  );
}
{% endhighlight %}

As you see, there is only one "statement" in this method: `assertEquals()`.

[Hamcrest](http://hamcrest.org/) with its
[`assertThat()`](http://hamcrest.org/JavaHamcrest/javadoc/2.0.0.0/org/hamcrest/MatcherAssert.html)
and a
[collection](http://hamcrest.org/JavaHamcrest/javadoc/2.0.0.0/allclasses-frame.html)
of basic matchers is a perfect instrument to make our
single-statement test methods even more cohesive and readable.

There are a number of practical benefits of this principle, if we
agree to follow it:

  * **Reusability**. The classes we will have to create for test
    assertions will be reusable in other test methods and test cases.
    Just like in the example above the `ArrayFromRandom` can be used
    somewhere else. Similar, Hamcrest matchers may and will constitute
    a library of reusable test components.

  * **Brevity**. Since it will be rather difficult to create a long
    test method with just a single `assert`, you and your fellow programers
    will inevitably write shorter and more readable code.

  * **Readability**. With a single `assert` it will always be obvious
    what the intent of the test method is. It will start with the intent
    declaration while all other lower level details will be indented.

  * **Immutability**. It will be almost impossible to have
    [setters]({% pst 2014/sep/2014-09-16-getters-and-setters-are-evil %}) in the
    production code if test methods have no place for algorithmic code. You
    inevitably will create
    [immutable objects]({% pst 2014/jun/2014-06-09-objects-should-be-immutable %})
    to make them testable with a single `assert`.

The biggest achievement we get if this principle is applied to our tests
is that they become declarative and object-oriented, instead of being
algorithmic, imperative, and procedural.

