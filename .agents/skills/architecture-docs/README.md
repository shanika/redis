# tecture — cross-agent architecture skill

Author and maintain a file-based architecture at `./architecture/` — `manifest.json`, one JSON file per diagram, and one markdown description per node. Includes JSON schemas and a self-contained Node validator.

Works across any agent that supports the open SKILL.md standard — Claude Code, GitHub Copilot (VS Code/CLI/cloud), Cursor, Cline, Windsurf, Codex CLI, and others.

## Install

### One-liner (recommended)

Using the [skills.sh](https://skills.sh) CLI — installs into any agent you target:

```bash
npx skills add tecture-io/tecture-skill
```

Or with [GitHub CLI](https://cli.github.com) (≥ 2.90.0):

```bash
gh skill install tecture-io/tecture-skill
```

Both auto-detect your installed agents and wire the skill into the right directory.

### Manual install

Clone into your coding agent's skill directory:

| Agent                          | Install path                         |
| ------------------------------ | ------------------------------------ |
| Claude Code                    | `~/.claude/skills/tecture`           |
| GitHub Copilot (VS Code / CLI) | `~/.copilot/skills/tecture`          |
| Cursor                         | `~/.cursor/skills/tecture`           |
| Cline, Windsurf, Codex, others | `~/.agents/skills/tecture` (neutral) |

```bash
git clone https://github.com/tecture-io/tecture-skill.git <chosen-path>
```

For a team-wide install, clone under the repo's project-scoped equivalent instead (e.g. `.claude/skills/tecture`, `.github/skills/tecture`, `.agents/skills/tecture`) and commit.

Restart the agent. The skill activates when you ask it to create, update, split, or document an architecture as local JSON/Markdown files.

## Run the validator

From your project root (where `./architecture/` lives), replace `<install-path>` with whichever of the paths above you chose:

```bash
node <install-path>/scripts/validate.mjs
```

Exit codes: `0` success, `1` validation failure, `2` internal error. Pass a path to validate somewhere other than `./architecture`.

## What it does

- C4 levels 1–3 with cross-diagram drill-down via `subDiagramId` (slug-based, no UUIDs).
- Mermaid blocks inside per-node markdown for runtime / sequence detail.
- JSON Schema (Draft 2020-12) validation: shape, reference integrity, cycle detection, global node-id uniqueness, orphan-description warnings.
- No runtime dependencies.

See [`SKILL.md`](SKILL.md) for the full guide, [`reference/schema.md`](reference/schema.md) for the schema, and [`reference/example/`](reference/example/) for a minimal working architecture.

## Source of truth

This repo mirrors [tecture-io/tecture](https://github.com/tecture-io/tecture) at `.claude/skills/tecture/`. Open issues and PRs against the main repo.

## License

MIT — see [LICENSE](LICENSE).
