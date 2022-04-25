---
layout: content
tags: essay
title: Reliability is a User Experience
---

# Reliability is a User Experience

_This post is mainly intended for developers and service operators, but
hopefully there is something in it for anyone who cares about reducing
mistakes._

As reliability-minded developers, we are responsible for maintaining a reliable
service that our customers can depend on. This usually means writing safe code,
creating dependable tests, and making sensible decisions when planning our next
big feature release. But sometimes, prioritizing reliability means putting on
our designer hats and looking at our problems from a different perspective.

Today, let's talk about how the reliability of your service starts and ends with
a user experience, intentionally or not.

-----

For the context of this article, let's borrow this (admittedly, rather broad)
definition of User Experience (UX) from
[Caglar Araz and the UX Collective][ux-definition]...

> User experience refers to the singular and accumulated experiences that occur
> for users as a consequence of them interacting with an object in a given
> context.

So what does that have to do with operating reliable services? Well, a lot
actually.

Let's start with a bit more background. Many software developers tend to
associate the terms user experience with user interface, and for good reason:
most of the people with UX in their job description are working directly with
user interfaces. Using our handy UX definition from above, let's imagine a
common scenario...

> A customer (the user) is interacting with a search component (the object)
> because they want to find and delete a document they had previously created
> (the context).

What might a good user experience look like here?

> The customer wastes no time and is able to quickly and easily locate, verify,
> and delete their desired document using an easy-to-locate search bar, a
> memorable search phrase, and two clicks.

What might a bad user experience look like here?

> The customer struggles to find the search component and has to ask a friend
> for help. Then every search they attempt brings up 15 documents that look
> nothing like theirs. Finally, they locate one with a title that looks
> familiar and accidentally delete another person's work. 

So what controls the actual outcome of the user experience? Since we can't
control the user or the goal they are trying to accomplish, the best way to
persuade the experience in a positive direction is through design improvements
to the user interface. Painting in broad strokes, this is why companies hire
designers.

Back-end developers should keep in mind that this concept doesn't only apply to
graphical interfaces. The lens of user experience maps directly to command line
applications and API consumption as well. Like really, when was the last time
you created a symbolic link without having to look up the correct ln syntax?

So then how exactly does this affect reliability? I promise we're getting there!

Let's take our example above and apply it a bit more liberally. What if we
replaced the concrete artifact of a UI with a more abstract idea, such as a
READ-DO checklist? This might sound a bit strange, but hear me out.

> A developer (the user) is following a deployment checklist (the object)
> because they want to release a new feature into production (the context).

Hopefully this is starting to feel a bit more real. So what might a good user
experience look like here?

> The developer is able to follow a short, easy checklist that allows them to
> safely deploy the new feature without having to worry about silly mistakes.

You probably see where I'm going with this. Now for a bad user experience.

> The developer struggles to follow a long, partially outdated checklist that
> leaves production in a state of disrepair while reporting a successful
> deployment to the developer, who is now leaving for a month-long trip to
> Antarctica.

Okay, I'll admit it, I am taking a bit of poetic license here; but, you see what
I'm saying right? In the same way that a customer has a user experience when
using a UI or command-line tool, a developer has a UX while following a
checklist. And exactly the same as before, if we want to sway the user
experience in a positive direction, our best bet is to improve the object of
interaction: the checklist.

-----

"What if I don't have a checklist like that?" you might ask.

"Well, you're wrong," I would reply.

All teams have checklist-like processes, steps of sequential actions that they
perform when trying to accomplish a specific goal. Yours might not look like a
written deployment checklist. In fact, it might not be a checklist at all, but
maybe just a loose ordering of steps. It might be a short list with a few steps,
or a long complex one with branching paths. It might be explicitly written out
in a shared document, or it might live entirely in your head. Regardless of the
form it takes, any regular process is, in its most basic form, just a checklist.

Therefore, every process used by your team has a user experience to consider.

For any one of your team's processes, consider what a good user experience might
look like. What might a bad user experience look like? What aspects of your
process might sway it one way or the other? What could you change about the
process to tip the odds in a positive direction?

You will be amazed what you find broken when you start examining the user
experience element of your team's various processes. Sometimes UX problems are
purely technical, like a flaky test script or an outdated dependency causing
frustration. But in many cases, the problems can have a much more human
component, such as a ridiculously long command that everyone runs incorrectly,
or an emergency runbook that is hidden somewhere deep in the bowels of some
Confluence space. Imagine if the runbook author were camping far from
civilization and the incident-response team couldn't find the document.

-----

_For illustration's sake, let's look at one UX scenario my team ran into
recently. For context, we make reliability-minded tools and services that are
used internally by the rest of our company._

My team kept seeing bugs appearing in our production deployments, and they were
usually fairly stupid ones. The whole user experience of releasing a change was
miserable because we often had to waste precious development time performing
rollbacks and investigations that could have been avoided. Clearly, things
needed to improve.

Our first response was to add some extra scrutiny to the code reviews that were
already a part of our release process. More eyes and more time probably did
improve overall code quality, but the added review didn't seem to decrease the
number of bugs that crept into production.

The next idea was to add a manual testing step to our release process. One of my
teammates created a runbook that guided the change developer through a
comprehensive testing procedure on our staging environment. We all agreed that
the runbook should help us catch most bugs before they reached production, but
practice found us routinely ignoring the runbook out of impatience or
frustration, rendering any of the expected benefits null and void.

"Well," we wondered, "If none of us want to run the full test, could we just
have a script do the hard work?" Yes, actually. Using a cron-like scheduler, we
began performing regular tests of our staging environment which exercised the
"happy path" of our application. We could simply deploy our changes to the
staging environment and wait for the test to pass a few times without raising
alarms. If nothing broke, we could be reasonably confident that the core
functionality of our service was working correctly. Would that finally fix our
bug problem?

Jackpot! With alarms sounding for any core defects appearing in our staging
environment, we started catching most of our bugs before production. Because we
had an automated test that performed the bulk of our testing workload, we felt
more comfortable digging deeper into specific focus areas and let the automation
handle all of the tedious toil. And now, after our long journey, we finally had
a reliable and satisfying release experience, and all was right and happy in the
world!

Well, almost. Our release experience isn't quite perfect yet, as evident by the
changes we tend to leave "soaking overnight" in staging. (Then, of course, we
forget them in the morning like that one university roommate we don't talk to
anymore). We are still designing some potential solutions that discourage this
bad habit, but overall, our team is much happier with the reliability of our
service today than we were three months ago.

-----

Now it's your turn. Can you think of any tools or processes that your team
regularly struggles to use effectively? Looking at them as a user experience
might help you get your reliability SLIs trending in the right direction.

Not sure of a UX to start with? Here's a few questions to get your creativity
flowing.

Do we have high-confidence in the changes we release to customers?  Do we have
any vulnerabilities or outdated dependencies from weeks ago that still haven't
been fixed?  Do we keep writing the same code over and over to fix the same
problems again and again?  Do we make it easy for new teammates to start working
on our services?  Do we have any sections of code that we are afraid to touch?

Changing any UX starts with a conversation. Sit down with a few teammates and
ask, "How do we do X?" Try to talk through each of the steps you would perform
when interacting with a tool or process. As you go, pay close attention to steps
that seem confusing, prove time-consuming, or spur strong memories. At the end,
review all of the steps you performed as well as any notes you may have taken,
then ask, "Are we happy with how we accomplished our goal?" If not, you probably
have one or more steps that need to be added, changed, or even removed.

While you are walking through a user experience, it can be helpful to keep these
questions in mind...

Can anyone on the team use this tool or process, or is the knowledge limited to
only a few people?  Is it obvious how to use this tool or process, or will the
user need some kind of outside help?  Is it easy to make mistakes, or are there
safeguards that help prevent them from happening?  Is it straightforward to do
things "the right way", or is it often easier to employ risky shortcuts?  Is it
obvious what your actions are doing, or do you only see the end result of a
hidden sequence of events?  In what kinds of situations or emotional states
would users be performing these actions?  How do we know that we completed the
process successfully?  Could a computer be doing this for us, or should it be
done "by hand"?

By intentionally considering the interactions you make with a tool or process,
and by asking yourself the right questions for your context, you will find ways
to improve your user experience. This might make the difference between a
release process people hate or one they don't have to think about at all. It
could be the difference between a debugging tool that raises service quality
across the company or yet another tool that nobody knows/cares about.


[ux-definition]: https://uxdesign.cc/we-have-lost-track-of-what-ux-actually-means-8d55259dacb0
