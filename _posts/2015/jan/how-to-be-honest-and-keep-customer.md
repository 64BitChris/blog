---
layout: post
title: "How to Be Honest and Keep a Customer"
date: 2015-01-05
tags: mgmt
description:
  Most software outsourcing companies don't show their source
  code until the product is ready, keeping their customers in the dark and
  increasing the risk of project failure; it's a bad practice.
keywords:
  - source code
  - software demonstration
  - software development outsourcing
  - how to outsource software development
  - software development outsourcing
---

Most of our clients are rather surprised when we explain to them that
they will have full access to the source code from the first day
of the project. We let them see everything that is happening in
the project, including the Git repository, bug reports, discussions
between programmers, continuous integration fails, etc.
They often tell me that other software development outsourcing teams
keep this information in-house and deliver only final releases,
rarely together with the source code.

I understand why other developers are trying to hide as much as possible.
Giving a project sponsor full access to the development environment
is not easy at all. Here is a summary
of problems we've been having and our solutions. I hope they help you
honestly show your clients all project internals and still keep them on board.

<!--more-->

{% picture /images/2014/12/99-francs.png 0 99 francs (2007) by Jan Kounen %}

## He Is Breaking Our Process

This is the most popular problem we face with our new clients. Once they
gain access to the development environment, they try to give instructions
directly to programmers, walking around
[our existing process]({% pst 2014/oct/2014-10-06-software-project-lifecycle %}).
"I'm paying these guys; why can't I tell them what to do?" is a very typical mindset.
Instead of submitting requests through our standard change management mechanism,
such a client goes directly to one of the programmers and tells him what
should be fixed, how, and when. It's micro-management in its worst form.
We see it very often. What do we do?

First, we try to understand why it's happening. The simplest answer
is that the client is a moron. Sometimes this is exactly the case, but it's a rare one. Much more
often, our clients are not that bad. What is it, then? Why can't they follow
the process and abide by the rules? There are a few possible reasons.

Maybe the **rules are not explained well**.
This is the most popular root cause &mdash; the rules of work are not clear
enough for the client. He just doesn't know what he is supposed to do in order
to submit a request and get it implemented. To prevent this, we try to educate
our clients at the beginning of a new project. We even write guidance
manuals for clients. Most of them are happy to read them and learn the
way we work, because they understand that this is the best way to achieve
success while working with us.

Maybe our **management is chaotic**, and the client is trying to "organize"
us by giving explicit instructions regarding the most important tasks. We've
seen it before, and we are always trying to learn from this. As soon as
we see that the client is trying to micro-manage us, we ask
ourselves: "Is our process transparent enough? Do we give enough
information to the client about milestones, risks, plans, costs, etc.?"
In most cases, it's our own fault, and we're trying to learn and improve.
If so, it's important to react fast, before the client becomes too agressive in his
orders and instructions. It will be very difficult to escort him back to the normal
process once he gets "micro-management" in his blood.

Maybe the client is not busy enough and has **a lot of free time**, which he
is happy to spend by giving orders and distracting your team. I've seen
this many times. A solution? Keep him busy. Turn him into a member of the
team and assign him some tasks related to documentation and research. In
my experience, most clients would be happy to do this work and help the
project. Well, at least they think they are helping :)

## He Is Asking Too Much

A technically-savvy client can turn the life of
[an architect]({% pst 2014/oct/2014-10-14-who-is-software-architect %})
into a nightmare by constantly asking him to explain every
single technical decision made, from "Why PostgreSQL instead of MySQL?"
to "Why doesn't this method throw a checked exception?" Constantly answering
such questions can turn a project into a school of programming.
Even though he is paying for our time, that doesn't mean we should teach him
how to develop software, right? On the other hand, he is interested in knowing
how _his_ software is developed and how it works. It's a fair request, isn't it?

I believe there is a _win-win_ solution to this problem. Here is how we
manage it. First of all, we make all his requests formal. We ask
a client to create a new ticket for each request, properly explaining
what is not clear and how much detail is expected in the explanation.

Second, we look at such requests positively &mdash; they
are good indicators of certain inconsistencies in the software. If it's
not clear for the client why PostgreSQL is used and not MySQL, it's a fault
of [our architect]({% pst 2014/oct/2014-10-14-who-is-software-architect %}).
He didn't document his decision and didn't explain
how it was made, what other options were considered, what selection criteria
were applied, etc. Thus, a request from a client is [a bug]({% pst 2014/apr/2014-04-13-bugs-are-welcome %})
we get for free. So, we look at it positively.

Finally, we charge our clients for the answers given. Every question, submitted
as a ticket, goes through the full flow and
[gets billed]({% pst 2014/oct/2014-10-21-incremental-billing %})
just as any other ticket. This
approach prevents the client from asking for too much. He realizes that we're
ready to explain anything he wants, but he will pay for it.

## He Is Telling Too Much

This problem is even bigger than the previous one. Some clients
believe they are savvy enough to argue with
[our architect]({% pst 2014/oct/2014-10-14-who-is-software-architect %})
and our programmers about how the software should be developed. They don't just
ask why PostgreSQL is used, they tell us that we should use MySQL,
because "I know that it's a great database; my friend is using it, and his
business is growing!" Sometimes it gets even worse, when suggestions
are directed at every class or even a method, like "You should use
a Singleton pattern here!"

Our first choice is to agree and do what he wants. But it's a road to nowhere.
Once you do it, your project is ruined, and you should start thinking
about a divorce with this client. Your entire team will quickly turn into
a group of coding monkeys, micro-managed by someone with some cash. It's a very
wrong direction; don't even think about going there.

The second choice is to tell the client to mind his own business and let
us do ours. He hired us because we're professional enough to develop the
software according to his requirements. If he questions our capabilities,
he is free to change the contractor. But until then, he has
to trust our decisions. Will this work? I doubt it. It's the same as
giving him the finger. He will get offended, and you won't get anything.

The solution here is to turn the client's demands into project requirements.
Most of them will be lost in the process, because they won't be sane enough
to form a good requirement. Others will be documented, estimated, and crossed-out
by the client himself, becuase he will realize they are pointless or too expensive. Only
a few of them will survive, since they will be reasonable enough. And they will
help the project. So it is also a _win-win_ solution.

For example, he says that "you should use MySQL because it's great". You tell
him that the project requirements document doesn't limit you to choose whichever
database you like. Should it? He says yes, of course! OK, let's try to
document such a requirement. How will it sound? How about, "We should only
use great databases?" Sound correct? If so, then PostgreSQL satisfies this
requirement. Problem solved; let us continue to do our work. He will have a
hard time figuring out how to write a requirement in a way that disallows
PostgreSQL but allows MySQL. It is simply not possible in most cases.

Sometimes, though, it will make sense; for example, "We should use a database server
that understands our legacy data in MySQL format". This is a perfectly sane
requirement, and the only way to satisfy it is to use MySQL.

Thus, my recommendation is to never take a client's demands directly to execution,
but rather use them first to amend the requirements documentation. Even if you don't
have such documentation, create a simple one-page document. Agree with the
client that you work against this document, and when anyone wants to change
something, you first have to amend the document and then have your team ensure the
software satisfies it. This kind of discipline will be accepted by any client
and will protect you against sudden and distracting corrections.

## He Is Questioning Our Skills

When source code is open to the client, and he is technically capable
of reading it, it is very possible that one day he will tell us that
our code is crap and we have to learn how to program better. It has not
happened in our projects for many years, but it has happened before,
when we weren't using static analysis as a [mandatory step]({% pst 2014/aug/2014-08-13-strict-code-quality-control %})
in our continuous integration pipeline.

Another funny possibility is when the client shows the source code to a "friend",
and he gives a "professional" opinion, which sounds like, "They don't know what they
are doing." Once such an opinion hits your client's ears, the project is at
a significant risk of closure. It'll be very difficult, almost impossible,
to convince the client not to listen to the "friend" and continue
to work with you. That's why most outsourcers prefer to keep their sources
private until the very end of the project, when the final invoice is paid.

I think that an accidental appearance of a "friend" with a negative opinion
is un-preventable. If it happens, it happens. You can't avoid it.
On the other hand, if you think your code is perfect and your team
has only talented programmers writing beautiful software, this is not
going to protect you either. An opinion coming from a "friend" won't be objective;
it will just be very personal, and that's why it's very credible. He is a friend
of a client, and he doesn't send him bills every week. Why would he lie?
Of course, he is speaking from the heart! (I'm being sarcastic.) So, no matter
how beautiful your architecture and your source code is, the "friend" will
always be right.

In my opinion, the only way to prevent such a situation or minimize its
consequences is to organize regular and systematic independent technical
reviews. They will give confindence to the client that the team
is not lying to him about the quality of the product and key technical
decisions made internally.

<hr/>

To conclude, I strongly believe it is important to be honest
and open with each client, no matter how difficult it is. Try to
learn from every conflict with each client, and improve your management
process and your principles of work. Hiding source code is not
professional and makes you look bad in the eyes of your clients and
the entire industry.

