---
layout: post
title: "Why InputStream Design Is Wrong"
date: 2016-04-10
place: Washington, D.C.
tags: java oop
description:
  Class InputStream in Java has three methods read(),
  this is what I think is very wrong. Not only in
  this class, but in general in OOP.
keywords:
  - why InputStream is a class
  - InputStream
  - InputStream java
  - method overloading
  - method overloading java
book: elegant-objects 2.9
---

It's not just about `InputSteam`, but this class is a good
example of a bad design. I'm talking about three overloaded
methods `read()`. I've mentioned this problem in Section 2.9
of [Elegant Objects](/elegant-objects.html). In a few words,
I strongly believe that interfaces must be "functionality poor".
`InputStream` should have been an interface in the first place
and it should have had a single method `read(byte[])`. Then, if
its authors wanted to give us extra functionality, they should have
been created supplementary "smart" classes.

<!--more-->

This is how it looks now:

{% highlight java %}
abstract class InputStream {
  int read();
  int read(byte[] buffer, int offset, int length);
  int read(byte[] buffer);
}
{% endhighlight %}

What's wrong? It's very convenient to have an ability to read
a single byte, an array of bytes, or even an array of bytes
with a direct positioning into a specific place in the buffer!

However, we are still lacking a few methods: for reading that bytes and
immediately saving into a file, converting to a text with a selected
encoding, sending them by email, and posting on Twitter. Would be
great to have that features too, right in the poor `InputStream`.
I hope Oracle Java team is working on them now.

In the mean time, let's see what exactly is wrong with what these
bright engineers designed for us already. Or maybe let me show
how I would design `InputStream` and we'll compare:

{% highlight java %}
interface InputStream {
  int read(byte[] buffer, int offset, int length);
}
{% endhighlight %}

This is my design. The `InputStream` is responsible for reading
bytes from the stream. There is one single method for this
feature. Is it convenient for everybody? Does it read and post
on Twitter? Not yet. Do we need that functionality? Of course we do,
but it doesn't mean that we will add it to the interface. Instead,
we will create supplementary "smart" class:

{% highlight java %}
interface InputStream {
  int read(byte[] buffer, int offset, int length);
  class Smart {
    private final InputStream origin;
    public Smart(InputStream stream) {
      this.origin = stream;
    }
    public int read() {
      final byte[] buffer = new byte[1];
      final int read = this.origin.read(buffer, 0, 1);
      final int result;
      if (read < 1) {
        result = -1;
      } else {
        result = buffer[0];
      }
      return result;
    }
  }
}
{% endhighlight %}

Now, we want to read a single byte from the stream? Here is how:

{% highlight java %}
final InputStream input = new FileInputStream("/tmp/a.txt");
final byte b = new InputStream.Smart(input).read();
{% endhighlight %}

The functionality of "reading a single" byte is outside of `InputStream`,
because this is not its business. The stream doesn't need to know
how to manage the data after they are read. All the stream
is **responsible** for is reading, not parsing or manipulating afterwards.

Interfaces must be small.

Obviously, method overloading in interfaces is a code smell. An interface
with more that **three methods** is a good candidate for refactoring. If methods
overload each other &mdash; it's a serious trouble.

Interfaces must be small!

You may say that the creators of `InputStream` cared about performance, that's
why allowed us to implement `read()` in three different forms. Then, I have
to ask again, why not creating a method for reading and immediately posting
on Twitter? That will be fantastically fast. Isn't it what we all want?
A fast software which nobody has any desire to read or maintain.
