The database layer: the dictionaries that hold every key, the API the type handlers use to read and write them, and the expiration and notification machinery that rides each access.

## Responsibilities
- Provide the keyspace API — `lookupKeyRead`/`lookupKeyWrite`, `dbAdd`/`dbOverwrite`/`setKey`, `dbDelete` — over the per-database key and expires stores
- Select databases (`selectDb`), scan the keyspace (`SCAN`/`KEYS`), and lazily expire keys on access (`expireIfNeeded`, `setExpire`, `getExpire`)
- Emit keyspace/keyevent Pub/Sub notifications (`notifyKeyspaceEvent`) and flag watched/dirty keys (`signalModifiedKey`, `server.dirty`)

## Key files
- `src/db.c` — the keyspace and database API, expiration, `SCAN`, dirty/watch signaling
- `src/notify.c` — keyspace/keyevent notification emission

## Dependencies
- **Inbound** — called by `data-type-commands` for every key read/write
- **Outbound** — stores and returns values from `object-model` (`robj`/`kvobj`); the `server.dirty` it bumps is what drives propagation to persistence and replication one level up

## Tech Stack
- C; per-`redisDb` dict/kvstore for keys and a parallel store for expirations
