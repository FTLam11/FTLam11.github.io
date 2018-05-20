---
layout: post
title:  RubyElixirConf Taipei 2018 Day 2
date:   2018-05-20
tags: [ruby]
---

This is a follow up post to my first Ruby conference ever, I'll share some notes, thoughts, and
things I learned for each talk I attended.

# Inspecting and Crafting Rails by Akira Matsuda

This talk was pretty awesome, one of my favorites. The premise of the
talk was an honest assessment of Rails' speed compared to other web
development frameworks. The first "benchmark" from a reputable source showed Rails running in
production mode being ~5x slower than Pheonix in development mode.
Thought not having great expertise in performance, honestly these
benchmarks can be pretty misleading. There's so many variables to
control at the application level and even more at other parts.

* Different metrics for measuring performance: Time elapsed, method
calls, and object allocation
* Matsuda rendered `Hello Taipei` using Pheonix/Rails at
7us/652us respectively
* Found that ActionView is taking the longest time
* Why does rending plain text with no template take so long?
`default_render` is called, there are a grand total of 244 methods
called for render plain
* Memory: Memory usage is doubled with introduction of a template, lots
of string duplication. Suspect `activesupport/output_safety.rb` as
starting point for investigation after looking through stack trace.
* Matsuda encourages everyone to try improving Rails, we all can do our
part to improve and evolve Rails performance. Tag him in a pull request
for Rails!

## Quote of the day

This is gold. Back in the day it was called pulling a Fronk. Push
straight to master baby! Shieeeeeeeeeeeeeeeeeeeeeeeet.

### As a Rails committer I don't need to make pull requests for small changes that are obvious. Oops, I broke `master` (branch of Rails) yesterday, I'm sorry!" - Akira Matsuda

# High Concurrent Ruby Web Development Without Fear by Delton Ding

This talk was in Mandarin, and was about implementing high concurrency
using a framework called Midori. I only understood about ~85% of the Mandarin, but about 30 minutes
into the talk, there was some conversation between the audience and
speaker about [Fiber](https://www.slideshare.net/KoichiSasada/fiber-in-the-10th-year). I got lost after this.

# Building Serverless Ruby Bots by Damir Svrtan

This was a more tangible/practical talk about implementing Ruby bots on
AWS lambda (free tier) despite the platform not supporting Ruby.

* Traveling Ruby, M Ruby, and J Ruby were each used to write a bot that
web scraped an apartment rental website, found the newest listings, and
sent an email of new listings
* The primary constraints for running on the free tier were code size,
memory consumption, and execution time
* [Petition FaaS providers to support
Ruby](https://www.serverless-ruby.org/)

# Real Life: Acceptance Testing in a Rails Application by Manic Chuang

Writing requirements is painful. Writing software requirements is
self-defeat. I've never seen so much rock hunting in my life as I have
in my ~1 year as a web developer.

* Common spec sources: Project wiki, github README, word doc. Honestly
terrible sources of truth, they can get outdated very easily/have
duplication/have conflicting views
* People leave/join the team - personnel turnover is brutal if there
isn't infrastructure/processes in place to recover quickly
* PM's don't like using Cucumber? Will generally gravitate towards what
they are already comfortable using to document requirements

Manic provided a few key points for acceptance testing:

* Let PMs/Issue owners lead
* Developers are just supporters
* Spec style is determined by PMs/Issue owners
* Best practice is not the point
* Currently using Rspec in documentation format to record specs...
* Issue numbers are recorded directly in the tests

Well, given that best practices are to be ignored, I still don't think
it's a good idea to put acceptance level specs in your Rspec unit tests.
Acceptance level specs are user level requirements. Rspec specs are
functional level requirements. But if time is a crunch... as long as
they are documented somewhere...

# How Ruby has dramatically improved the productivity of developers in
Rakuten Travel by Tetsushi Awano

Essentially this was a talk about how the DevOps team at Rakuten Travel
used Ruby to make a bunch of internal applications to help automate a
lot of their responsibilities. Due to the size of Rakuten, number of
features/teams, there was a lot of silo'ing and minimal sharing of
information/duplication in effort. By integrating information and
providing a common interface to all the different teams, developers can
spend more time actually writing features than trying to setup multiple server
environments over and over again. Tesushi seems like a cool guy, is a
biker that has done the 環島 twice and also raps. I asked for his top 3
rappers all time, but unfortuntately I don't much about Japanese
hip-hop. Shame on me.

# Rails loves React

I was hoping for more information about how easy Rails 5 has made
integrating React, but in this talk webpacker was discouraged. I don't
know why. Didn't seem like anyone in the audience cared, so I guess
everyone is running Rails strictly in API mode and not mixing ERB with
React. Oh well.

# Day 2 Final Thoughts

It's been a while since I worked on a personal project. It was
encouraging to talk with other attendees about Ruby and share stories. I
think it's about time to deep dive into some of the most commonly used
gems. Hopefully the next time I attend a RubyConf I'll be a speaker.
