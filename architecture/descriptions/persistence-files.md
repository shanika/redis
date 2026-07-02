The on-disk artifacts that let a server reconstruct its dataset after a restart.

## Responsibilities
- Hold compact RDB snapshots for fast full reloads and for seeding replicas
- Hold the append-only file directory, replaying logged writes for finer-grained durability

## Tech Stack
- Binary RDB format and AOF (base RDB/AOF + incremental files in `appendonlydir/`), written by the server's persistence engine
