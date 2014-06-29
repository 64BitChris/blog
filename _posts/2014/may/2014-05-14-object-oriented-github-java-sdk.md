---
layout: post
title: "Object-Oriented Github API"
date: 2014-05-14
tags: github jcabi
description:
  Since none of the existing Java APIs are truly object-oriented
  or elegant enough for our quality standards, we
  decided to create a new one with the best principles of OOP in mind.
keywords:
  - github api
  - github java api
  - github object oriented java api
  - best github java api
  - java api github
  - java github api
  - object oriented github java api
  - java api design
---

{% badge http://img.yegor256.com/2014/05/github-logo.png 128 %}

[Github](http://www.github.com) is an awesome platform for maintaining Git sources and
tracking project issues. I moved all my projects (both private and public)
to Github about three years ago and have no regrets. Moreover,
Github gives access to almost all of its features through RESTful JSON API.

There are [a few](https://developer.github.com/libraries/)
Java SDKs that wrap and expose the API. I tried to use them,
but faced a number of issues:

 * They are not really object-oriented (even though one of them has a description that says it is)
 * They are not based on JSR-353 (JSON Java API)
 * They provide no mocking instruments
 * They don't cover the entire API and can't be extended

{% badge http://img.jcabi.com/logo-square.png 64 %}

Keeping in mind all those drawbacks, I created my
own library &mdash; [jcabi-github](http://github.jcabi.com).
Let's look at its most important advantages.

<!--more-->

## Object Oriented for Real

Github server is an object. A collection of issues is an object,
an individual issue is an object, its author is an author, etc.
For example, to retrieve the name of the author we use:

{% highlight java %}
Github github = new RtGithub(/* credentials */);
Repos repos = github.repos();
Repo repo = repos.get(new Coordinates.Simple("jcabi/jcabi-github"));
Issues issues = github.issues();
Issue issue = issues.get(123);
User author = new Issue.Smart(issue).author();
System.out.println(author.name());
{% endhighlight %}

Needless to say, `Github`, `Repos`, `Repo`, `Issues`, `Issue`,
and `User` are interfaces. Classes that implement them are not visible in the library.

## Mock Engine

`MkGithub` class is a mock version of a Github server. It behaves
almost exactly the same as a real server and is the perfect
instrument for unit testing. For example, say that you're
testing a method that is supposed to post a new issue to Github
and add a message into it. Here is how the unit test would look:

{% highlight java %}
public class FooTest {
  @Test
  public void createsIssueAndPostsMessage() {
    Github github = new MkGithub("jeff");
    github.repos().create(
      Json.createObjectBuilder().add("name", owner).build()
    );
    new Foo().doTheThing(github);
    MatcherAssert.assertThat(
      github.issues().get(1).comments().iterate(),
      Matchers.not(Matchers.emptyIterable())
    );
  }
}
{% endhighlight %}

This is much more convenient and compact than traditional
mocking via [Mockito](https://code.google.com/p/mockito/) or a similar framework.

## Extendable

It is based on JSR-353 and uses jcabi-http for HTTP request
processing. This combination makes it highly customizable and extendable,
when some Github feature is not covered by the library (and there are many of them).

For example, you want to get the value of `hireable` attribute of a `User`.
Class `User.Smart` doesn't have a method for it. So, here is how you would get it:

{% highlight java %}
User user = // get it somewhere
// name() method exists in User.Smart, let's use it
System.out.println(new User.Smart(user).name());
// there is no hireable() method there
System.out.println(user.json().getString("hireable"));
{% endhighlight %}

We're using method `json()` that returns an instance of
[`JsonObject`](http://docs.oracle.com/javaee/7/api/javax/json/JsonObject.html)
from JSR-353 (part of Java7).

No other library allows such direct access to JSON objects
returned by the Github server.

Let's see another example. Say, you want to use some feature
from Github that is not covered by the API. You get a `Request`
object from `Github` interface and directly access the HTTP entry point of the server:

{% highlight java %}
Github github = new RtGithub(oauthKey);
int found = github.entry()
  .uri().path("/search/repositories").back()
  .method(Request.GET)
  .as(JsonResponse.class)
  .getJsonObject()
  .getNumber("total_count")
  .intValue();
{% endhighlight %}

[jcabi-http](http://http.jcabi.com) HTTP client is used by [jcabi-github](http://github.jcabi.com).

## Immutable

All classes are truly immutable and annotated with
[`@Immutable`](http://aspects.jcabi.com/annotation-immutable.html).
This may sound like a minor benefit, but it was very important for me.
I'm using this annotation in all my projects to ensure my classes are truly immutable.

## Version 0.8

A few days ago we released the latest [version 0.8](https://github.com/jcabi/jcabi-github/releases/tag/jcabi-0.8).
It is a major release, that included over 1200 commits. It covers the entire Github API and is
supposed to be very stable.
The library ships as a JAR dependency in [Maven Central](http://repo1.maven.org/maven2/com/jcabi/jcabi-github)
(get its latest versions in [Maven Central](http://search.maven.org/)):

{% highlight xml %}
<dependency>
  <groupId>com.jcabi</groupId>
  <artifactId>jcabi-github</artifactId>
</dependency>
{% endhighlight %}
