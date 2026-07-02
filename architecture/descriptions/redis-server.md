The main Redis daemon: an event-driven process that holds the entire keyspace in RAM and serves all client, peer, and background traffic, executing commands single-threaded so each runs atomically. It is the host that boots, owns the global state, and coordinates the six subsystems detailed at the next level down (networking, command/keyspace, persistence, replication, cluster, scripting & modules).

## Responsibilities
- **Boot & lifecycle** — `main()` loads config via `loadServerConfig()`, runs `initServer()` (event loop, signal handlers, shared objects, databases, listeners), then rehydrates the dataset from RDB/AOF with `loadDataFromDisk()`; `prepareForShutdown()` persists and drains on `SIGTERM`/`SIGINT`
- **Drive the main loop** — `aeMain()` over a pluggable backend, with `serverCron()` (hz-paced timer) running the housekeeping: client timeouts (`clientsCron`), hash-table resizing (`databasesCron`), active expiry (`activeExpireCycle`), stats, and scheduling of `BGSAVE`/AOF-rewrite; `beforeSleep()`/`afterSleep()` bracket each loop iteration for AOF flushing and eviction
- **Orchestrate command execution** — `processCommand()` gates every command (auth/ACL, arity, OOM, cluster redirect, replica read-only) and `call()` invokes the handler, then propagates dirty writes to the AOF and replicas via `propagateNow()`, updates the slow log, monitors, and stats
- **Own global state** — the singleton `redisServer` struct holds clients, databases, replication and cluster state, the AOF buffer, configuration, and counters; all subsystems read and mutate it
- **Manage memory & forks** — `zmalloc` over jemalloc with `maxmemory` eviction (`performEvictions()`); `redisFork()` spawns child processes for RDB save and AOF rewrite, coordinated over the child-info pipe
- **Host extensibility** — loads `.so` modules at startup (`moduleLoadFromQueue()`) and runs the embedded Lua VM, surfacing both back through `call()` so their writes replicate correctly

## Key files
- `src/server.c` — process entry, main loop, `serverCron`, `processCommand`/`call`, propagation, signals
- `src/server.h` — the `redisServer` struct and shared declarations
- `src/config.c` — config-file and `CONFIG GET/SET` parsing
- `src/evict.c`, `src/expire.c` — `maxmemory` eviction and key expiration driven from the cron/command paths
- `src/zmalloc.c`, `src/bio.c` — allocator wrapper + OOM handling, and background I/O worker threads

## Dependencies
- **Inbound** — `redis-cli`, `redis-benchmark`, and application clients send RESP over TCP/Unix/TLS listeners (`initListeners()`); `redis-sentinel` monitors it and triggers failover
- **Outbound** — writes RDB snapshots and the AOF to the local filesystem; streams to `peer-nodes` via replication (`replicationFeedSlaves`) and the cluster bus; loads and invokes `loadable-modules` through the Module API (`RM_Call`, timers, keyspace hooks)

## Tech Stack
- C; entry point `src/server.c`
- Pluggable event backends (`ae_epoll`/`ae_kqueue`/`ae_evport`/`ae_select`), bundled jemalloc allocator (`deps/jemalloc`) and embedded Lua VM (`deps/lua`)
- Internal data structures: sds, dict, adlist, rax, quicklist, listpack; HDR histogram for latency stats
