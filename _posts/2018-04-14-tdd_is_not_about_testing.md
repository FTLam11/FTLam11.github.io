---
layout: post
title:  TDD is not about testing, it's about design
date:   2018-04-14
tags: [testing, work culture]
---
This week I had a conversation about writing tests with my boss and
another coworker. When I first onboarded, our company had seven Rails
developers working on 2-3 projects simultaneously. We had some request
specs to test an API for one project.

A couple months later I was tasked with integrating a payment service. I
started with writing unit tests to help me guide the **design** of the
service. It wasn't a complicated object to test, just a couple public
methods to massage the data payload and a callback function to process
the bank response. While writing tests by definition tests the
functionality of the system under test, I prefer to emphasize the
overall positive impact it had on how I went about designing the
service. Having the benefit of being confident about new code not
breaking existing code was a nice add-on.

Fast forward to the present where our team is now six Rails developers
working/maintaining 10+ projects there has been more bugs and burden. I
can't say I understand how our company manages projects and schedules,
but I do know that it is vastly different from what I've experienced
back in my days of being a fed.

Cost, schedule, performance. Pick any two. That has always been the
mantra for project management. With overtime pay rarely being enforced
in Taiwan, the two prioritized factors are always cost and schedule. I
ain't gonna front, I haven't written tests for several entire projects.
The rest of our Rails team hasn't either.

I feel the project management philosophy is strange. At times I am
bamboozled, bufuddled, and confused. Are we really just gonna gamble and
hope the end user doesn't use this particular functionality or interact
with the system in a certain way?

I'm certain there is some shady stuff going on behind the scenes. What
is our mission? How can we have more projects than engineers? It's 2018
and cash still rules everything around me.

In any case, straight up jumping into TDD with little or no prior
experience is something one simply does not do. There is a learning
curve. Our current waterfall approach to system development is not
cutting it. We haven't written enough mature libraries to be at that
point yet. The story goes on and on.
