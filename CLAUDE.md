# CLAUDE.md

Environment and workspace layout for this repository. `[CUSTOMIZE]` every section below for your machine and project.

> **All agents**: read `AGENTS.md` first — universal entry point with config boundaries and read order.

## Environment

- **OS**: `[CUSTOMIZE — e.g. Windows 11, macOS, Linux]`
- **Shell**: `[CUSTOMIZE — e.g. PowerShell 5.1, zsh, bash]`
- **Shell note**: `[CUSTOMIZE — e.g. on PowerShell 5.1, && chaining is unavailable; use "; if ($?) { ... }" instead]`
- **Language/runtime versions**: `[CUSTOMIZE — e.g. Node v24 / npm v11, Python via a specific launcher]`
- **Git user**: `[CUSTOMIZE]`

## Workspace Layout

`[CUSTOMIZE]` — describe where things live. If this workspace spans multiple root paths (e.g. a cloud-synced docs root plus a local fast-I/O code root), document the split and the rationale, for example:

| Path pattern | Use for | Rationale |
|---|---|---|
| `[CUSTOMIZE: cloud-synced root]` | Docs, specs, research, deliverables | Survives machine loss, shareable |
| `[CUSTOMIZE: local root]` | Code repos, git projects, heavy I/O | Faster, no sync overhead |

An initiative/project may span both. Each sub-project should carry its own `CLAUDE.md` with scoped context (goals, stack, conventions) — read it in addition to this file when working inside that scope.

## Codebase Intelligence (optional)

`[CUSTOMIZE — if you use a codebase-graph / onboarding tool, document the entry commands and where the generated artifacts should be committed here.]`

## Jupyter / Notebooks (optional)

`[CUSTOMIZE — if this workspace includes notebooks, note the language, the dataset location convention, and any expected cell-flow pattern (imports → config → load → transform → visualize → export).]`
