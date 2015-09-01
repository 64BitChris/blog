---
layout: post
title: "Redundant Variables Are Pure Evil"
date: 2015-09-01
tags: oop java
description:
  Despite good intentions, redundant variables actually
  make your code dirty and difficult to understand.
keywords:
  - redundant variables
  - too many variables
  - variables in oop
  - variables in java
  - java variables
---

A redundant variable is one that exists exclusively
to **explain** its value. I strongly believe that such a variable is
not only pure noise but also **evil**, with a very negative effect
on code readability. When we introduce a redundant variable, we intend to make our code
cleaner and easier to read. In reality, though, we make it more verbose
and difficult to understand. Without exception, any variable used only
once is redundant and must be replaced with a value.

<!--more-->

{% picture /images/2015/09/y-tu-mama-tambien.jpg 0 Y Tu Mamá También (2001) by Alfonso Cuarón %}


Here, variable `fileName` is redundant:

{% highlight java %}
String fileName = "test.txt";
print("Length is " + new File(name).length());
{% endhighlight %}

This code must look differently:

{% highlight java %}
print("Length is " + new File("test.txt").length());
{% endhighlight %}

This example is very primitive, but I'm sure you've seen these
redundant variables many times. We use them to "explain" the code &mdash;
it's not just a string literal `"test.txt"` anymore but a `fileName`.
The code looks easier to understand, right? Not really.

Let's dig into what "readability" of code is in the first place. I think this
quality can be measured by the number of seconds I need to understand the
code I'm looking at. The longer the timeframe, the lower the readability.
Ideally, I want to understand any piece of code in a **few seconds**. If I can't,
that's a failure of its **author**.

[Remember]({% 2015/jun/2015-06-29-simple-diagrams %}),
if I don't understand you, it's your fault.

An increasing length of code degrades readability. So the more variable
names I have to remember while reading through it, the longer
it takes to digest the code and come to a conclusion about
its purpose and effects. I think **four** is the maximum number
of variables I can comfortably keep in my head without thinking
about quitting the job.

New variables make the code longer because they need extra lines to
be declared. And they make the code more complex because its reader
has to remember more names.

Thus, when you want to introduce a new variable to explain what your code is
doing, stop and think. Your code is too complex and long in the first place!
Refactor it using new objects or methods but not variables. Make your
code shorter by moving pieces of it into new classes or private methods.

Moreover, I think that in [perfectly designed methods]({% pst 2015/aug/2015-08-18-multiple-return-statements-in-oop %}),
you won't need **any** variables aside from method arguments.
