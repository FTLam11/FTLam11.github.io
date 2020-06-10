---
layout: post
title:  Investigative Reports - NoSQL DBs
date:   2020-01-12
tags: [database]
---
Over the past couple days, I've been surveying two NoSQL databases for
storing simple chat messages: MongoDB and Cassandra. This [Discord
Engineering blog
post](https://blog.discordapp.com/how-discord-stores-billions-of-messages-7fa6ec7ee4c7)
was a great read, explaining how their situation and usage influenced
selecting a suitable data store. I was curious about the sharding
concerns and decided to first investigate MongoDB in detail. But first,
how does replication work in MongoDB?

# MongoDB

## Replication

Props to the MongoDB docs, they are an enjoyable read, I get some
serious Protoss vibes. I digress... Data replication involves copying
data to provide higher availability. When shit happens we have data
backups to continue operation.

Data replication is implemented by MongoDB using *replica sets*. A
*replica set* consists of a group of long-running
[mongod](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod)
working together to handle requests, access, and manage background
operations. MongoDB defines three distinct member roles:

* A **primary** receives *ALL* write operations
* **Secondaries** replicate write operations from the primary, resulting
in identical data.
* An **arbiter** functions simply as a voter when electing a new primary

Therefore, only the primary and secondaries have data responsibilities.
When a primary goes down, an election is held to determine the new
primary. Since the arbiter does not hold any data, it cannot be elected
as primary, and solely acts as a voter to ensure an odd total number of
voters. The recommended replica set configuration is three members minimum
that hold data responsibilities.

## Sharding

* Risks when sharding too late
[pitfalls](https://blog.yugabyte.com/overcoming-mongodb-sharding-and-replication-limitations-with-yugabyte-db/)
* Picking correct sharding key is critical
* Prefer sharding keys with high cardinality (variance)
* Low cardinality causes loading between shards to become uneven, some
chunks may become too big to split
* How to balance this with queries across multiple shards?
* How is the hashing function chosen to encourage balanced writes to different shards?
- Hashed vs ranged sharding -> tradeoffs between writes + reads
* Ultimate goal is to have balanced shards in terms of writes (and to
minimize inter-shard queries?)

> Careful consideration in choosing the shard key is necessary for
> ensuring cluster performance and efficiency. You cannot change the
> shard key after sharding, nor can you unshard a sharded collection

### Sauces

* [Replica Set Members](https://docs.mongodb.com/manual/core/replica-set-members/)
* [Sharding](https://docs.mongodb.com/manual/sharding/)
