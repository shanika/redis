The local disk where Redis persists its dataset for durability and restart recovery.

## Responsibilities
- Store point-in-time RDB snapshot files
- Store the append-only file (AOF) directory of logged write commands

## Tech Stack
- POSIX filesystem; files written by the persistence engine (RDB `.rdb`, AOF `appendonlydir/`)
