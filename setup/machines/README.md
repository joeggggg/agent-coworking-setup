# Machine Manifests

`[OPTIONAL — delete this folder if the workspace only ever lives on one machine.]`

One file per machine: `<MACHINE-NAME>.md`. Find the name with `$env:COMPUTERNAME`
(Windows) or `hostname` (macOS/Linux).

## Why one file per machine

A workspace shared across machines — especially one inside a cloud-synced folder —
accumulates facts that are true on one machine and false on the next: mount points,
runtime versions, what is installed where.

Recording those in a **shared** inventory file guarantees conflict copies the moment two
machines edit it. One file per machine means two machines never write the same file.

## Rules

| Rule | Why |
|------|-----|
| Only edit the manifest for the machine you are on | You cannot see another machine's disk |
| Manifests are documentation, not config | No tool reads them. Machine-local *config* lives in `<USER>`, outside the synced folder |
| Portable facts do not belong here | Persona, conventions, commands are the same everywhere — they belong in `AGENTS.md` / `CLAUDE.md` / `.claude/` |
| Re-scan rather than copy | When adding a machine, copy an existing file for shape, then verify every line against real disk |

See `AGENTS.md` § *Multi-Machine & Sync Safety*.

---

## Template

```markdown
# <MACHINE-NAME> — Machine Manifest

> **Scanned**: <DD-MMM-YYYY> against live disk

## Paths

| Token | Resolves to | Status |
|-------|-------------|--------|
| `<WS>` | `[workspace root on this machine]` | |
| `<LOCAL>` | `[local code root]` | present / absent |
| `<USER>` | `[user home]` | |

## Toolchain

| Tool | Version here |
|------|--------------|
| Node / npm | |
| Python | |
| Shell | |
| OS | |

## Agent config — user-global (not synced)

| Path | State |
|------|-------|
| `.claude/CLAUDE.md` | present / absent |
| `.claude/settings.json` | present / absent |
| `.claude/commands/` | count |

## Notes

- Anything true of this machine and not the others.
```
