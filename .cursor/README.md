# `.cursor/` — Cursor Configuration

`[OPTIONAL — delete this folder if Cursor isn't in your toolchain.]`

## What Cursor actually reads

| Customization | Project path | User path | Format |
|---|---|---|---|
| **Rules** | `.cursor/rules/*.mdc` | — | YAML frontmatter (`description`, `globs`, `alwaysApply`) + markdown |
| **Rules (universal)** | `AGENTS.md` at repo root | — | Plain markdown — Cursor reads this too |
| **Skills** | `.cursor/skills/<name>/SKILL.md` | `~/.cursor/skills/` | YAML frontmatter (`name`, `description`) + markdown |
| **Subagents** | `.cursor/agents/*.md` | `~/.cursor/agents/` | YAML frontmatter + markdown body = system prompt |
| **Commands** | `.cursor/commands/*.md` | `~/.cursor/commands/*.md` | Markdown. Being superseded by Skills |
| **Hooks** | `.cursor/hooks.json` + `.cursor/hooks/*` | `~/.cursor/hooks.json` | JSON config + scripts over stdin/stdout |
| **MCP servers** | `.cursor/mcp.json` | — | JSON |

Project paths win over user paths when names collide.

⚠️ **`.cursor/hooks/` alone does nothing** — hooks are registered in `.cursor/hooks.json`;
the directory only holds the scripts that file points at.

⚠️ **There is no `.cursor/permissions.json`.** Agent allowlists are set in Cursor
Settings → Agents (Run Mode), not by a file in the repo. A committed `permissions.json`
is inert.

## Rule format

```markdown
---
description: One line — when this rule applies
globs: ["**/*.ts"]        # omit for always-on rules
alwaysApply: true          # false = model decides from `description`
---

Rule body in markdown.
```

Prefer **one always-on entry-point rule** that points at `AGENTS.md`, plus narrow
`globs`-scoped rules for language- or directory-specific standards. Duplicating the whole
persona into `.mdc` files guarantees drift.

## Skills

If a skills package is installed at `.claude/skills/`, mirror it here as **bridge entries**
— a `SKILL.md` with the same `name` and `description` frontmatter whose body points at the
canonical file — rather than copying content. Same triggers, one source of truth.
