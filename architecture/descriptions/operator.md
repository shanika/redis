A human administrator (or automation) that deploys, configures, and monitors a Redis instance or cluster.

## Responsibilities
- Tune `redis.conf`, apply `CONFIG SET`, and manage ACL users
- Observe health via `INFO`, `MONITOR`, slow log, and latency tooling
- Drive cluster setup and maintenance (slot assignment, reshard, failover)

## Tech Stack
- `redis-cli`, configuration files (`redis.conf`, `sentinel.conf`), `CLUSTER`/`CONFIG`/`ACL` commands
