The live request path inside `redis-server`: everything that turns a client's bytes on a socket into an executed command and a reply. Groups the three subsystems a command flows through — networking, command/keyspace execution, and the embedded script/module engines it can invoke.

## Responsibilities
- Accept connections and parse the RESP protocol off the wire (`event-loop-networking`)
- Dispatch each command, enforce ACLs, and mutate the in-memory data types (`command-keyspace`)
- Run user Lua scripts, functions, and module command handlers in-process (`embedded-execution`)

## Tech Stack
- C, with an embedded Lua VM; single-threaded command execution over the shared event loop
