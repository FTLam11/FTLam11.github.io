---
layout: post
title:  Cheggin out FP languages
date:   2018-04-16
tags: [elixir]
---
Checked out the latest and greatest developer roadmap and noticed being
recommended to learn a functional language. Read through a lot of
opinions on which language to choose and I'm considering both Elixir and
Clojure. Let's take some notes and see how this goes. I hope these
resources are good.

### Resources

* joyofelixir
* Elixir School
* braveclojure

### Observations

* `iex` is the REPL for Elixir
* Symbols are called Atoms
* String interpolation is also done via `#{}`
* Arrays are called **lists**, hashes are called **maps**
* Ugh why do maps need to start with `%`
* Functions care about argument arity
* Elixir file extension is `.exs`

OK, let's try and break shit

{% highlight elixir %}
iex(1)> "lol".length
** (CompileError) iex:1: invalid call "lol".length()

iex(1)> String.length("lol")
3

iex(2)> .14
** (SyntaxError) iex:5: syntax error before: '.'

iex(2)> String.length('lol')
** (FunctionClauseError) no function clause matching in
String.Unicode.length/1

The following arguments were given to String.Unicode.length/1:

# 1
'lol'

Attempted function clauses (showing 1 out of 1):

def length(string) when is_binary(string)

(elixir) lib/elixir/unicode/unicode.ex:270: String.Unicode.length/1

iex(2)> 5 % 2
** (SyntaxError) iex:7: syntax error before: '%'

iex(2)> rem(5,2)
1

iex(3)> yo = %{a: 1, b: 2}
%{a: 1, b: 2}
iex(4)> yo[:a]
1
iex(4)> yo["a"]
nil

iex(4)> yell = fn (phrase) -> String.upcase(phrase) end
#Function<6.99386804/1 in :erl_eval.expr/5>
iex(5)> yell.("who daT?")
"WHO DAT?"
{% endhighlight %}
