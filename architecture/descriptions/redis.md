An in-memory data structure store serving as a database, cache, message broker, and streaming engine, accessed by clients over the RESP wire protocol.

## Responsibilities
- Hold the keyspace in RAM and execute commands against rich data types (strings, hashes, lists, sets, sorted sets, streams, bitmaps, HyperLogLog, geo)
- Provide durability through RDB snapshots and the append-only file
- Scale and stay available via primary/replica replication and Redis Cluster sharding

## Tech Stack
- C (`src/server.c` is the entry point)
- RESP protocol over TCP, Unix sockets, and TLS
