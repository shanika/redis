Other `redis-server` instances this server connects to for replication and cluster membership.

## Responsibilities
- Serve as replicas of, or primaries to, this server via the replication stream
- Exchange slot ownership and failure-detection gossip over the cluster bus

## Tech Stack
- Redis replication protocol (`PSYNC`/`REPLCONF`) and the cluster-bus binary protocol on the `+10000` port
