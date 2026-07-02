The data-management half of `redis-server`: everything that keeps the dataset durable on disk and shared across nodes. Groups the three subsystems fed by command execution — persistence, replication, and clustering.

## Responsibilities
- Snapshot and log the dataset to disk so it survives restarts (`persistence-engine`)
- Keep replicas in sync with their primary via the replication stream (`replication-engine`)
- Shard the keyspace by hash slot and maintain membership over the cluster bus (`cluster-engine`)

## Tech Stack
- C; RDB/AOF on-disk formats, the `PSYNC` replication protocol, and the gossip cluster bus
