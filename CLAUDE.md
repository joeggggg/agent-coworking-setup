# CLAUDE.md

Environment and workspace layout for this repository. `[CUSTOMIZE]` every section below for your machine and project.

> **All agents**: read `AGENTS.md` first — universal entry point with config boundaries and read order.

## Environment

- **OS**: `[CUSTOMIZE — e.g. Windows 11, macOS, Linux]`
- **Shell**: `[CUSTOMIZE — e.g. PowerShell 5.1, zsh, bash]`
- **Shell note**: `[CUSTOMIZE — e.g. on PowerShell 5.1, && chaining is unavailable; use "; if ($?) { ... }" instead]`
- **Language/runtime versions**: `[CUSTOMIZE — e.g. Node v24 / npm v11, Python via a specific launcher]`
  ⚠️ A version recorded here is true of **the machine it was written on**. If this file is
  shared across machines, verify with `node -v` / `python --version` before relying on it,
  and record per-machine values in `setup/machines/<MACHINE-NAME>.md` instead of editing
  this line. See `AGENTS.md` § *Multi-Machine & Sync Safety*.
- **Git user**: `[CUSTOMIZE]`

## Workspace Layout

`[CUSTOMIZE]` — describe where things live. If this workspace spans multiple root paths (e.g. a cloud-synced docs root plus a local fast-I/O code root), document the split and the rationale, for example:

| Token | Path pattern | Use for | Rationale |
|---|---|---|---|
| `<WS>` | `[CUSTOMIZE: cloud-synced root]` | Docs, specs, research, deliverables | Survives machine loss, shareable |
| `<LOCAL>` | `[CUSTOMIZE: local root]` | Code repos, git projects, heavy I/O | Faster, no sync overhead |

Refer to these roots by **token**, not by absolute path — `<WS>/docs/`, not
`D:\Cloud\project\docs\`. `<WS>` resolves to the directory containing `AGENTS.md`.
Cloud clients let each machine pick its own mount point, so a drive letter correct on one
machine is wrong on the next. `<LOCAL>` **may not exist** on the current machine — check
before assuming a project's code half is present.

⚠️ **If `<WS>` is inside a cloud-synced folder, it has no machine-local tier.** The sync
client replicates everything beneath it and ignores `.gitignore`, so machine-specific
config must live in `<USER>` (outside the synced folder), never in `<WS>`. Full rules in
`AGENTS.md` § *Multi-Machine & Sync Safety*.

An initiative/project may span both. Each sub-project should carry its own `CLAUDE.md` with scoped context (goals, stack, conventions) — read it in addition to this file when working inside that scope.

## Codebase Intelligence (optional)

`[CUSTOMIZE — if you use a codebase-graph / onboarding tool, document the entry commands and where the generated artifacts should be committed here.]`

## Jupyter / Notebooks (optional)

`[CUSTOMIZE — if this workspace includes notebooks, note the language, the dataset location convention, and any expected cell-flow pattern (imports → config → load → transform → visualize → export).]`
