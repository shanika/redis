The per-type command handlers: the implementations behind every data-type command, one file per type, that read arguments and apply the operation to a value in the keyspace.

## Responsibilities
- Implement the command handlers for each type — strings, lists, sets, hashes, sorted sets, streams, and arrays (`setCommand`, `lpushCommand`, `zaddCommand`, `xaddCommand`, …)
- Extend the type surface with bitmaps (`bitops.c`), HyperLogLog (`hyperloglog.c`), and geo (`geo.c`)
- Choose and transition the right value encoding for each type as it grows

## Key files
- `src/t_string.c`, `t_list.c`, `t_set.c`, `t_hash.c`, `t_zset.c`, `t_stream.c`, `t_array.c` — the type handlers
- `src/bitops.c`, `src/hyperloglog.c`, `src/geo.c` — additional command surfaces over string/zset values

## Dependencies
- **Inbound** — invoked by `command-dispatch` via the command table's `proc` pointer
- **Outbound** — calls `keyspace-db` to fetch/store/delete keys (`lookupKeyWrite`, `setKey`, `dbDelete`) and `object-model` to construct and encode values (`createObject`, `createStringObjectFromLongLong`)

## Tech Stack
- C; type-specific encodings — listpack, quicklist, intset, skiplist, and rax — selected per value
