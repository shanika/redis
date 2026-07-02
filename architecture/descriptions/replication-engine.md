The primary/replica subsystem that keeps replicas in sync with their primary.

## Responsibilities
- Perform full sync (RDB transfer) and partial resync from the replication backlog
- Stream the replication command flow to connected replicas
- Track replica acknowledgements for `WAIT` and failover safety

## Tech Stack
- C; `src/replication.c` implementing the `PSYNC`/`REPLCONF` protocol
