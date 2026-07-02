An application or service that stores and queries data in Redis through a client library or `redis-cli`.

## Responsibilities
- Issue RESP commands to read and write keys, run scripts, and subscribe to pub/sub or streams
- Manage its own connection pool and handle MOVED/ASK redirects in cluster mode

## Tech Stack
- Any Redis client library (hiredis and dozens of language bindings) speaking RESP2/RESP3
