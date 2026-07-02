The extensibility layer that runs user and third-party code inside the server process.

## Responsibilities
- Execute `EVAL` scripts and registered `FUNCTION` libraries on the embedded Lua VM
- Expose the C module API so loadable modules can add commands, types, and hooks
- Mediate these executions back through the command dispatch path with proper replication

## Tech Stack
- C and Lua; `src/script.c`, `src/script_lua.c`, `src/functions.c` (scripting) and `src/module.c` (`redismodule.h` API)
