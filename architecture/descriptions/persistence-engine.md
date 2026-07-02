The durability subsystem that snapshots the dataset and logs writes so it survives restarts.

## Responsibilities
- Fork and serialize the keyspace into RDB snapshots
- Append executed write commands to the AOF and perform background AOF rewrites
- Offload fsync and file closing to background I/O threads

## Tech Stack
- C; `src/rdb.c`, `src/aof.c`, and the background-job system in `src/bio.c`
