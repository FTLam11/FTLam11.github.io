---
layout: post
title:  Trix are 4 kidz
date:   2019-11-19
tags: [recursion]
---
In preparation for my n00bie talk on Elixir, I looked a bit into the
history of programming languages and was surprised. It was
interesting how the use cases of programming evolved over time, from
primarily solving math/science/engineering problems to
business/economics/finance to
web/media/communication/shopping/everything haha.

I figured I'd look into some of Ruby's language inspirations and set out
on naively checking out Lisp, specifically Scheme. My resource of choice was the famous
"Structure and Interpretation of Computer Programs". I forgot how much I
loathed college engineering text books, so verbose and always exuding weird pompous
overtones. I did check out some of the video lectures recorded back
sometime in the 1970s/80s and maaaaaannn did I get a good chuckle.
Straight up, the intro frames featured a wizard with some gnarly
mystical background music.

I decided to go back to "Little Schemer" and do the exercises in a
Racket console instead of pen + paper. The format for "Little Schemer"
is just so more easily digested IMO, because everything is practical.
The concepts and theory are demonstrated through working the exercises
and reinforced by the commandments. Hopefully I can finish the book this
time around.

I followed [this
article](https://crash.net.nz/posts/2014/08/configuring-vim-for-sicp/)
for setting up vim + Racket. The auto parenthesis setup is pretty nice,
but it's still a pain once you start using `cond` and have
super nested parens. Using the Racket REPL alone would be hell.

I think if I had to teach someone recursion from scratch, this is the
example I'd use to explain it:

1. What is `293993189 + 1239190499`?
2. Not sure that's a pretty larget number...
3. No sweat, how about `5 + 5`?
2. EZ dawg, `10`.
4. Nice, what about `6 + 4? 7 + 3? 8 + 2? 9 + 1`?
5. `10`, what you gettin at?
6. Wait, how about `10 + 0`?
7. `10`, but what's the point?
8. What is `293993189 + 1239190499`?

And this is how I'd explain why the example is relevant:

1. What is recursion?

> Recursion aims to: Take a problem that we might not really know how
> to solve off the bat, and slowly break it down into smaller problems
> that we can more easily solve. It involves calling the same function
> with slightly different arguments that get us closer to solving the
> problem.

2. What is the easiest problem to solve?

> The easiest problem to solve is one that doesn't need any solving,
> (Caveat: depends on if one is using tail or non-tail recursion, ignore for now)


3. If recursion revolves around breaking problems down into easier problems,
how do we know when to stop recursing?

> People often use the term base case to serve as a stop condition for
> recursion. In concrete terms, inputs like 0, "", and [] are what I
> like to call empty values. Empty values are awesome, because they can
> often signify that our work is done. Let's walk through an example:

```
a = 5
b = 5
a + b = ?

5 + 5 = 6 + 4 = 7 + 3 = 8 + 2 = 9 + 1 = 10 + 0
```

What we did was break `5 + 5` into successively easier problems to
solve. Once we got to `10 + 0`, notice that we didn't continue breaking
the problem down. We stopped because we hit the *base case*.

Let's try formalizing this discovery by defining a recursive function to
calculate the sum of two integers. Given two positive integers `a` and `b`,
return the sum.

```
function plus(a, b)
  if b is equal to zero
    then we done! let's return a
  else
    continue breaking problem down by recursing
```

The conditional could just as well be checking if `a` is zero, and
returning `b` instead, either way will work. So backtracking to solving
easy problems, what if we simplified integer problems into a single
conditional: Is the input **empty**? That's what it all really boils
down to.

If this conditional is `true`, then we are done. When the conditional
evaluates to `false`, we need to continue recursing by calling the same
function with arguments that get us closer to solving the problem. How
do we make integers become `0`? Let's subtract `1` from it, if we do
this operation enough, eventually we can hit `0`. With this approach, we
can churn out a more detailed function:

```
function plus(a, b)
  if b is equal to zero
    then we done! let's return a
  else
    plus(a, b - 1)
```

Hmm, but our function is still missing something. We no doubt are
breaking the problem into smaller ones by changing `b`. However, we
are not keeping track of `a`. Let's fix that by passing an updated `a`
as an argument.

```
function plus(a, b)
  if b is equal to zero
    then we done! let's return a
  else
    plus(a + 1, b - 1)
```

Cool, let's see our function in action:

```
plus(5, 5)
  5 == 0
    plus(6, 4)
      4 == 0
        plus(7, 3)
          3 == 0
            plus(8, 2)
              2 == 0
                plus(9, 1)
                  1 == 0
                    plus(10, 0)
                      0 == 0
                        10
```

What we just did was *tail recursion*. Tail recursion happens when the
last thing a function does is call itself. For more information check
out this [example](https://github.com/FTLam11/fronk-tolks/blob/master/elixir-intro.md#fibonacci-recursion-demo).

To sum things up, learning recursion can become a lot easier by focusing
on two things: identifying an empty value to use as a base case, and
figuring out what arguments to call on successive calls.
