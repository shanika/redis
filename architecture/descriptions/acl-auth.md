The authorization gate: it decides whether the connected user is allowed to run a command and touch the keys and channels it names.

## Responsibilities
- Maintain ACL users, passwords, and rule sets (`ACLCreateUser`, `ACLSetUser`, the `ACL` command family)
- Authorize each command via `ACLCheckAllPerm`/`ACLCheckAllUserCommandPerm` against command categories and key/channel patterns before dispatch proceeds
- Back `AUTH`/`HELLO` authentication and report the offending key/position on denial

## Key files
- `src/acl.c` — user model, rule parsing, and the per-command permission checks

## Dependencies
- **Inbound** — invoked by `command-dispatch` inside `processCommand()` before the handler runs
- **Outbound** — none; it is a pure check over the in-memory user table

## Tech Stack
- C; per-command ACL category bitmaps and glob key/channel patterns
