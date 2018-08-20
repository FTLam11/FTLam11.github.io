---
layout: post
title:  "Metaprogramming Ruby Part 3"
date:   2018-08-20
tags: [ruby]
---
Noting highlights from the book Metaprogramming Ruby (2nd Edition).

# Chapter 3. Tuesday: Methods

* Languages like Java and C use a compiler to check if the receiving
object for a method call has a matching method (static typing/static
language)

### Dynamic Dispatch

* Using `obj.send(method, args)` to call a method on an object instead
of using the usual dot notation
* The method simply becomes an argument, and is either a
string or symbol. Which method to pass in to `obj#send` can be
sent while the code is running.

### Dynamic Method

* Can define methods at runtime
* `Module#define_method` can be used to define methods at runtime instead of the usual `def` keyword

### method_missing

* Private instance method defined in `BasicObject` and inherited by all classes
* Overriding `method_missing`: takes method, arguments, and block

### Ghost Methods

* Instead of defining similar methods repeatedly, can respond to these method calls through `method_missing`
* When calling `respond_to?`, `respond_to_missing?` is called to check if the method is truly a ghost method.
* When overriding `method_missing`, should also override `respond_to_missing?`
* When the name of a ghost method conflicts with name of a real method, latter always wins
* Blank Slate: Can directly inherit from `BasicObject` to minimize chance of method name conflicts
