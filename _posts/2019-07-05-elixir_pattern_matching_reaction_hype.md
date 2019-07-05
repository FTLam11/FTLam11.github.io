---
layout: post
title:  Elixir pattern matching reaction hype
date:   2019-07-05
tags: [elixir]
---
Are you kiddings me? This is no yoke.

You can pattern match a function call!?

Consider this:

{% highlight elixir %}
{:ok, contents} = File.read("my_app.config")
{% endhighlight %}

This line 1) Reads `my_app.config` 2) if the return value pattern
matches successfully to `:ok`, then the file contents get assigned to
contents 3) if the match fails, i.e., the operation is not `:ok` then
an exception is raised.

Writing this in Ruby would maybe something like this:

{% highlight ruby %}
begin
  contents = File.read("my_app.config")
rescue StandardError => e
  # handle error here, can also rescue multiple error classes
end
{% endhighlight %}

But that ain't it folks, you can also pattern match arguments in
functions... **head explode**.

{% highlight elixir %}
defmodule Greeting do
  def hello({:english, name}) do
    "Hello #{name}"
  end

  def hello({:chinese, name}) do
    "你好 #{name}"
  end
end
{% endhighlight %}

`Greeting.hello/1` expects to be called with a tuple containing an
atom/name. Each definition of the same arity function is a clause,
together constituting the aptly named multiclause function.

But what if no clause matches? No matter, because a variable pattern
ALWAYS matches. That is hella dope. Guard clauses can be created by
simply matching against a variable. However, the order in which the
clauses are defined matters, since Elixir will try matching from top to
bottom. Hence guard clauses should be at the bottom.
