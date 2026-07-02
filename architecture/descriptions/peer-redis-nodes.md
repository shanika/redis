Other Redis server processes that this instance replicates with or shards a keyspace across.

## Responsibilities
- Act as replicas receiving the replication stream, or as primaries supplying it
- Participate in the Redis Cluster gossip bus to share slot ownership and node health

## Tech Stack
- Redis replication protocol (`PSYNC`) and the cluster bus binary protocol over a dedicated port
