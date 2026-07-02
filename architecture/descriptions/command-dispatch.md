The entry point of command execution: it resolves an incoming command name to a `redisCommand` table entry and drives it through the gate checks into its handler.

## Responsibilities
- Look up the command in `server.commands` (`lookupCommand`) and validate existence and arity
- Run the `processCommand()` gate — auth, ACL, OOM, cluster redirect, read-only-replica — then invoke `call()`, which executes `c->cmd->proc(c)`
- Carry command metadata (flags, key specs, ACL categories) generated into the command table

## Key files
- `src/commands.def`, `src/commands.c` — the generated command table and metadata
- `src/server.c` — `lookupCommand`, `processCommand`, and `call` (the dispatch and propagation orchestration)

## Dependencies
- **Inbound** — receives parsed argv from `event-loop-networking` (one level up)
- **Outbound** — calls `acl-auth` to authorize, then `data-type-commands` to run the handler

## Tech Stack
- C; the command table is generated from the JSON specs under `src/commands/`
