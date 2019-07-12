---
layout: post
title:  Elixir and BEAM SPOOKY
date:   2019-07-09
tags: [elixir]
---

![lol](https://pics.me.me/ending-a-recursive-function-ending-a-recursive-function-ending-a-38116677.png)

![LoL](https://poohbot.com/wp-content/uploads/2018/03/yo-dawg-recursion-recursion-825x510.jpg)

![LOL](https://img.memecdn.com/recursion_o_170485.jpg)

So startled...

Server processes in Elixir are long-running processes that handle
messages from other BEAM processes. They use a nifty tail recursion
feature to run indefinitely. Functions that call a function as the last
thing they do are called tail function calls.

Erlang treats tail function calls in a special manner. Normally a
function call would result in allocating another stack (
`stack level too deep` lol), but Erlang perform a
tail call optimization that prevents this. Instead, a "jump statement"
happens and no additional memory allocation occurs.

> It’s perfectly normal to have different functions from the same module
> running in different processes — there’s no special relationship
> between modules and processes. A module is just a collection of
> functions, and these functions can be invoked in any process.

lolwut, my noob face, one moar time for good measure tho

![LoL](https://poohbot.com/wp-content/uploads/2018/03/yo-dawg-recursion-recursion-825x510.jpg)
