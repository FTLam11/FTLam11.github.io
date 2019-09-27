---
layout: post
title:  K-N-O-W-L-E-D-G-E IS POWER
date:   2019-09-27
tags: [general]
---

# On computer processes vs threads

A process consists of serveral components:

* Program to be executed
* Register - data storage internal to CPU
* Counter/Pointer - a register (lol terminology) that tracks where a
computer is in its program sequence
* Stack - stores info of function calls (remember all those pretty error
stack dumps?)
* Heap - dynamically allocated memory (not CPU register) for the program
(all the variables, objects, etc... go here)

There can be multiple instances of a program, e.g., a rails server,
tmux, or an internet browser. Each instance is an process running
independently of other processes. This means the above components are
isolated to their respective process, so no inter-process sharing! It
would be heinous for one program to crash another one.

A thread is an execution unit. A process can have one or more
threads. When a process has a single thread, they are in effect one and
the same. Processes with multiple threads are more nuanced: they can
execute tasks concurrently by splitting up work and assigning it to
multiple threads.

In contrast to processes, threads **share** data (heap only, each thread
still has its own stack). Shared data has a significant benefit: it is
economical to create threads and communicate between them than it is to
create a whole new process (depends on use case). However, thread
safety can be a pain in the ass with all the mutating of shared state.
There are mechanisms like mutexes to synchronize concurrent mutation and
facilitate concurrent execution of code, but this path opens a whole
different Pandora's box so I digress.

### Sauces

1. [https://en.wikipedia.org/wiki/Process_(computing)](https://en.wikipedia.org/wiki/Process_(computing))
2. [https://www.quora.com/What-is-the-difference-between-register-and-memory](https://www.quora.com/What-is-the-difference-between-register-and-memory)
3. [https://stackoverflow.com/questions/3518445/why-are-cpu-registers-fast-to-access](https://stackoverflow.com/questions/3518445/why-are-cpu-registers-fast-to-access)
4. [https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/](https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/)
5. [https://www.toptal.com/ruby/ruby-concurrency-and-parallelism-a-practical-primer](https://www.toptal.com/ruby/ruby-concurrency-and-parallelism-a-practical-primer)
6. [https://dev.to/enether/working-with-multithreaded-ruby-part-i-cj3](https://dev.to/enether/working-with-multithreaded-ruby-part-i-cj3)
