---
layout: post
title:  Elixir Knowledge Dump
date:   2019-07-04
tags: [elixir]
---
Formally started learning Elixir today. Reading [Elixir in
Action](https://www.manning.com/books/elixir-in-action-second-edition)
first as recommended by one of my DBC instructors. Gonna dump a bunch of
first impressions and facts here.

### First impressions of erlang

* Originally created by Ericsson to develop telecom systems. Whoa. It's
features are primarily based around creating highly available, reliable,
concurrent systems.

* Brain proceeded to melt after looking at erlang for the first time. This
snippet from the book defines a server to sum numbers.

{% highlight erlang %}
-module(sum_server).
       -behaviour(gen_server).
       -export([
         start/0, sum/3,
         init/1, handle_call/3, handle_cast/2, handle_info/2, terminate/2,
         code_change/3
]).

start() -> gen_server:start(?MODULE, [], []).
sum(Server, A, B) -> gen_server:call(Server, {sum, A, B}).

init(_) -> {ok, undefined}.
handle_call({sum, A, B}, _From, State) -> {reply, A + B, State}.
handle_cast(_Msg, State) -> {noreply, State}.
handle_info(_Info, State) -> {noreply, State}.
terminate(_Reason, _State) -> ok.
code_change(_OldVsn, State, _Extra) -> {ok, State}.
{% endhighlight %}

* BEAM -> a single OS process consisting of schedulers and generally
many many Erlang processes

* An entire server platform can be developed exclusively in Erlang,
compare this to a Rails platform where we'd often see
Nginx/Rails/Redis/Cron/etc... That is pretty wild.

### Elixir

* Seems like it's all about Macros, a feature that at compile time
translates syntatically-sugared Elixir code into the more verbose form.
Hallelujah for less boiler-plate!

* At first glance, a lot of the built-in types have a pretty verbose
syntax compared to Ruby (yeah I'm biased)

* Maaaaaaan, it's been a rude awakening to functional programming so
far. I gotta call namespaced functions, even with the option of
aliasing/include that is a lot more typing.

* The idea of functions with the same name under the SAME namespace BUT
with different arities is kinda cool, especially using default values
so that lower arity functions can just delegate to higher arity ones. I
like the naming scheme for referring to functions, i.e.,
Module.function/arity.

* I gotta go back and review Little Schemer

* Was kinda shocked at first reading how lists are implemented as linked
lists. Got super baited into thinking these were arrays.

* No String type! A string is either a binary or list!!!!1!!!1!!

* So many types and their nuances/getting mixed up with habits from
Ruby...

* Module lookup process with atoms/compiled beam files is pretty interesting
