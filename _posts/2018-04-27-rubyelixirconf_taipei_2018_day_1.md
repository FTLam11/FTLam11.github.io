---
layout: post
title:  RubyElixirConf Taipei 2018 Day 1
date:   2018-04-27
tags: [ruby]
---

This was my first Ruby conference ever, I'll share some notes, thoughts, and
things I learned for each talk I attended.

# Ruby after 25 Years by Matz

* The other possible name for Ruby was Coral!
* Way back when Ruby 1.9 was released, there was a considerable divide
between the Ruby community between 1.8 and 1.9. Matz stated future
versions of Ruby will not introduce major breaking changes to avoid such
conflict.
* When asked about adding static typing to Ruby, Matz frowned and said
"I don't like type checking, it's not DRY". However he did list some
possible implementations like checking comments, a type declaration
file, or maybe automatic type inference.
* I forget the context, but testing was brought up somehow with Matz
responding "I hate tests, tests aren't DRY".
* Matz made a very ambitious statement with his initial Ruby3X3 plan,
being light-hearted and jokingly making no promises for whatever
proposed performance gains, but with the new JIT compiler scheduled for
release with Ruby 2.6 and several other performance boots planned, the
future of Ruby 3 looks good!

## Quote of the day

Check the following quote out. It dropped some jaws and rustled some
jimmies. I suppose it really enforces developers to test the shit out of
everything as well as embracing **fail fast**.

* "Our team only works on `master` branch, we always push
straight to production" - Harinskar P S of Red Panthers

# Distributing Ruby to the World by Hiroshi Shibata

* Mr. Shibata authored rbenv (pronounced r b env) and ruby-build
* "Official" Ruby sites = Matz controllable
* Ruby patches are mainly submitted via ruby-lang.org's redmine and
generally only for the trunk branch (main/newest)
* The Ruby core team is slowly making progress towards allowing pull
requests for larger changes to be made via GitHub
* For any Rubyists visiting Tokyo, check out [ Asakusa.rb
](https://asakusarb.esa.io/)

# Embracing Failures to Prevent Meltdown by Harisankar P S

* Circuit breakers are objects that have some function wrapped inside.
It monitors the success rate of the contained function, and will
activate itself when the failure rate exceeds a set threshold
* Failures must be defined correctly, e.g., a required action is not
being performed, a different result is observed, or a response is taking
too long
* Circuit breakers can be applied to "self-heal" an application. It has
three states:
  * Closed: System is working correctly
  * Open: System failures are too high, so respond with a cached
page/data, add the failed request to a retry queue, or show a nice error
page
  * Half-open: Self healing mode, try executing contained function at
  random intervals to ascertain failure

# Moving millions of dollars daily with Ruby while still being able to sleep at night by Sihui Huang

* "It's not about writing bug-free code. It's about knowing there will
be bugs and being ready to fix them" This is an interesting statement.
* The payments team at Gusto shared a number of best practices to help
identify and prioritize bugs faster:
  * Setup chat bot alerts with different levels of severity to notify
  on-call engineer and payments team
  * Raise errors often and as soon as possible, this allows for faster
  diagnose of where things are going wrong
  * Sidekiq job best practices included never saving state to a job. The
  job queue database can die, and there are issues with serializing
  objects so just pass the object id
  * Split large transaction amounts into smaller transactions. It's better
  for a couple of smaller transaction jobs to fail than one large amount
  transaction.
  * One query per job (one read + one write)
  * Jobs should be idempotent and transactional

# ActiveModel::Errors and AdequateErrors by lulalala

* Speaker did not like current implementation of ActiveMode::Errors
since the messages/details attributes are nested data structures, not
object-oriented, and can be hard to map since there isn't necessarily
1-1 correlation of messages to attribute (depends on how validations are
setup)
* AdequateErrors was created to make each model error a proper object

# Don't Repeat Your Code Review Suggestion by Soutaro Matsumoto

* Ever feel like you are making the same pull request comment over and
over again?
* Querly gem was created to DRY up comments for reviewing pull requests
* The gem differs from Rubocop in that it allows developers to add
custom rules very easily, this allows for more fine tuning that is
highly suitable to a team's needs
* Querly provides a DSL for writing rule patterns, users can define
rules in YAML format

# Boost Your Daily Development with Ruby's AST Toolchain by vzvu3k6k

This talk was kind of hard to follow. The speaker was trying to write
rules for Rubocop. He used AST's to trace different method calls and
find the correct attribute for an object. He used the `ripper` and `parser`
gems.

# Day 1 Final Thoughts

I'm pretty sure I was the most junior engineer that day. It was cool
getting to see Matz and other Ruby committers continue to show their
devotion to Ruby. 25 years is a very long time for any computer
language, but show must go on! Ruby has done so much for me, and I aim
to make future contributions using it.
