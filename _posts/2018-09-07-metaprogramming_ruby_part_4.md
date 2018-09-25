---
layout: post
title:  Metaprogramming Ruby Part 4
date:   2018-09-07
tags: [ruby]
---
Noting highlights from the book Metaprogramming Ruby (2nd Edition).

# Chapter 4. Wednesday: Blocks

* A block is a closure, it captures bindings of current environment and carries them around. It is NOT an object.

### Scope

* When program changes scope, some bindings are replaced by a new set of bindings
* Scope Gates: Places where a program leaves the previous scope behind and opens a new scope
  * Class definitions
  * Module definitions
  * Methods
* Flat Scope: Passing objects through scope gates

```ruby
Class.new do; end
Module.new do; end
define_method :holla do; end
```

### instance_eval and instance_exec

* `instance_eval` evaluates a block in the context of an object
* `instance_exec` is similar to `instance_eval` but can also pass arguments to block
* with `instance_eval` ivars switch on the context of self, e.g., the ivars of the caller fall out of scope once `instance_eval` switches the value of self to the receiver.
* `instance_exec` can be used to keep the bindings of the caller visible to the receiver

### Proc Objects

* Blocks can be converted to Procs for later execution via `Proc#call`
* There are five ways to declare Proc objects

```ruby
p = Proc.new { |x| x + 1 }
p = lambda { |x| x + 1 } (via Kernel)
p = proc { |x| x + 1 } (via Kernel)
p = ->(x) { x + 1 } (lambda)
&some_block
```
* `&` operator: block must be passed in as last argument to a method and prepended with & operator, converts the block to Proc

### method_missing

* Private instance method defined in `BasicObject` and inherited by all classes
* Overriding `method_missing`: takes method, arguments, and block

### Procs vs Lambdas

* Procs created with `lambda` are called lambdas, the rest are simply called procs
* Return statements in lambdas simply return from the lambda, while in procs a return statement returns from the scope where the proc itself is defined
* When using procs avoid explicit returns, returning from top-level scope fails with `LocalJumpError: unexpected return`
* Lambdas check arity, will fail with `ArgumentError` if arity is wrong
* Procs fits argument list to its own expectations, it will drop excess arguments and assign `nil` to missing arguments
* Rubyists tend to use lambdas, they behave more like methods since they care about arity and return keyword behavior is the same

### Method Objects

* A method is also a callable object
* `Kernel#method` and `Kernel#singleton_method` can store a certain method as an object to be used by `Kernel#call` later on
* Two handy methods to convert between method/proc: `Method#to_proc` (method to proc), `define_method` (proc to method)
* Key difference between method objects and the other callable objects: method objects are evaluated in scope of its object, while callable objects are evaluated in the scope where they are defined

### Unbound Methods

* `UnboundMethod#bind` and `Module#instance_method` can bind an unbound method to an object
* Methods that come from a class can only be bound to same class or subclass, methods originating from module do not have this limitation
* `Module#define_method` can also be used
* Methods can become unbound from their class/module and be used to create normal callable methods by using `Method#unbind` or `Module#instance_method`
* ActiveSupport autoloading feature uses this unbound method trick if a particular class does not want to autoload itself

```ruby
module Loadable
  def self.exclude_from(base)
    base.class_eval { define_method(:load, Kernel.instance_method(:load) }1
  end
end
```

Any class passed to `exclude_from` has the `load` method added to it.  This new `load` method comes as an `UnboundMethod` using `instance_method` to unbind `Kernel#load`.
