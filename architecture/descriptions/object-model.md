The value representation shared by every type: the `robj` object that wraps a value with its type, encoding, refcount, and LRU/LFU metadata, plus the helpers that create and convert them.

## Responsibilities
- Define `robj`/`kvobj` and construct values (`createObject`, `createStringObject`, `createStringObjectFromLongLong`, shared integer objects)
- Manage the encodings each type can take (`int`, `embstr`/`raw`, `listpack`, `intset`, `quicklist`, `hashtable`, `skiplist`, `stream`) and back `OBJECT ENCODING`/`REFCOUNT`
- Handle refcounting and memory accounting for stored values

## Key files
- `src/object.c` — object creation, encoding conversion, refcounting, and `OBJECT` introspection

## Dependencies
- **Inbound** — used by `data-type-commands` to build values and by `keyspace-db` to store/return them
- **Outbound** — none; it is the leaf data-representation layer

## Tech Stack
- C; tagged `robj` with type + encoding fields over the concrete structures (sds, listpack, quicklist, intset, skiplist, rax)
