---
layout: post
title:  Pattern matching in Elixir
date:   2018-04-20
tags: [elixir]
---
Whoa, haven't seen this technique used much in Ruby. I think I've seen
some pattern matching in ES6. Seems like it'd be useful for massaging
and operating on nested data structures. Some kind of combination
between `dig` and variable assignment.

Let's work through different uses of pattern matching using the
following list of maps:

%{ highlight elixir %}
iex(15)> hiphopheads = [%{name: 'Pharoahe Monch', group: 'Organized
Konfusion'}, %{name: 'Qtip', group: 'ATCQ'}, %{name: 'Imani', group: 'The Pharcyde'}, %{name: 'Irrelevant', group: 'Nickelback'}]

[
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'},
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
{% endhighlight %}

### Pattern matching lists

First and foremost, we gotta cut the crap by pattern matching like this:

{% highlight elixir %}
iiex(16)> [ok, atcq, tp | crap] = hiphopheads
[
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'},
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
iex(17)> ok
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'}
iex(18)> atcq
%{group: 'ATCQ', name: 'Qtip'}
iex(19)> tp
%{group: 'The Pharcyde', name: 'Imani'}
iex(20)> crap
[%{group: 'Nickelback', name: 'Irrelevant'}]
{% endhighlight %}

Sweet, what happened here was we assigned `ok, atcq, tp` to the first
three items in `hiphopheads`. Then the pipe operator `|` was used to
assign all remaining items in the `hiphopheads` list to `crap`.

We must be mindful of pattern matching more items than we actually have
in the list, i.e., if we desired to pattern match `ok, atcq, tp, crap, yo`
again `hiphopheads`. Elixir would respond with with `MatchError`.

### Pattern matching maps

What if we wanted to work with a specific group? Luckily, we can also
pattern match against maps. What's happening below is `ok["group"]` is assigned to `crew`, and `ok["name"]` is assigned to `nombre`.

{% highlight elixir %}
iex(24)> %{group: crew, name: nombre} = ok
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'}
iex(25)> crew
'Organized Konfusion'
iex(26)> nombre
'Pharoahe Monch'
{% endhighlight %}

### Combining pattern matching lists and maps together!!!!!

Holy shit this is pretty cool. Check this shit, gonna break it down step
by step. In this example below, we match the `hiphopheads` list to `ok`
and `rest` for a total of 2 pattern matches.

{% highlight elixir %}
iex(28)> [ok | rest] = hiphopheads
[
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'},
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
iex(29)> ok
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'}
iex(30)> rest
[
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
{% endhighlight %}

What if we further wanted to match the `group` key of the first match
`ok` against `hiphopheads`?

{% highlight elixir %}
iex(31)> [ok = %{group: squad} | rest] = hiphopheads
[
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'},
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
iex(32)> ok
%{group: 'Organized Konfusion', name: 'Pharoahe Monch'}
iex(33)> squad
'Organized Konfusion'
iex(34)> rest
[
%{group: 'ATCQ', name: 'Qtip'},
%{group: 'The Pharcyde', name: 'Imani'},
%{group: 'Nickelback', name: 'Irrelevant'}
]
{% endhighlight %}

The upshot is we gotta give the `=` a little more respect. Aside from
assignment, we can also use it to pattern match. Really dope shit.
