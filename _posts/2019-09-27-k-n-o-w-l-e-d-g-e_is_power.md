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

# On HTTP

* HTTP uses TCP standard to faciliate communication
* TPC is a network protocol that allows two hosts to connect and
exchange data streams. TCP guarantees order of data delivery.
* Request format - HTTP verb, resource path, protocol, headers, body
* Response format - protocol, status code, status message, headers, body
* HTTP messages will not be human readable anymore with HTTP/2 -
messages are put into a binary structure called **frame**.

### Caching

Caching is a vicious technique in which a copy of a resource is served
instead of the latest and greatest resource. For general stuff that
doesn't change much, this can be a great boon for servers, since clients
that request the cached resources won't need to make the full request
trip. The cached resource can be served directly from the client's cache
or by some intermediate proxy, speeding up response times and minimizing
network bandwidth.

The `Cache-Control` header has several directives that can be set by the
server to instruct clients/caches what requests may be cached.

* `no-store` - disallows caching
* `no-cache` - cache checks with server to see if it can respond with a cached copy
* `public` - response may be cached by any cache
* `private` - response may only be cached by a private cache
* `max-age=<seconds>` - the maximum time relative to the time of request
that a resource can be considered fresh
* `must-revalidate` - cache must check on freshness of resource so that
expired copies are not used

When a cached resource expires (note this does not imply that it is
stale), the resource must be fetched again or validated on subsequent
requests for it. The server may provide either a strong or weak
validator to facilitate cache validation.

The server response may contain the `ETag` header to represent a
specific version of the requested resource. On subsequent client
requests for the same resource, the `If-Match` or `If-None-Match`
headers containing the `ETag` value can be sent to check if the resource
is still fresh.

Alternatively the server response may utilize the `Last-Modified` header
as a weak validator, it is less accurate than the `ETag` header. Clients
may use the `If-Modified-Since` or `If-Unmodified-Since` headers along
with the `Last-Modified` value to check for freshness.

When a requested resource is still fresh the server responds with 304.
If the resource has been updated, it responds normally with the updated
resource/200.

### Sauces

1. [https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
2. [https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)
