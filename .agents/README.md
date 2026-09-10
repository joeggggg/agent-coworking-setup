# `.agents/` — Antigravity Workspace Customizations

`[OPTIONAL — delete this folder if Antigravity isn't in your toolchain.]`

This is the directory Antigravity actually discovers for workspace customizations. It
walks up from the current working directory to the repository root looking for `.agents/`
(and the accepted aliases `.agent/`, `_agents/`, `_agent/`).

## Structure

```text
.agents/
├── skills/<name>/SKILL.md    # Skills — YAML frontmatter: name, description
├── rules/*.md                # Additional rule files
├── plugins/<name>/plugin.json
├── hooks.json                # Lifecycle hooks
└── mcp_config.json           # MCP server definitions
```

## What goes where

| Customization | Path | Notes |
|---|---|---|
| **Rules** | `AGENTS.md` / `GEMINI.md` at the repo root, or `.agents/rules/*.md` | Root `AGENTS.md` is this repo's entry point and already carries the Antigravity rules |
| **Skills** | `.agents/skills/<name>/SKILL.md` | Progressive disclosure — only `name` + `description` are injected until the skill is activated |
| **Plugins** | `.agents/plugins/<name>/plugin.json` | Bundles related skills, rules and MCP config |
| **Hooks** | `.agents/hooks.json` | Runs commands at agent lifecycle points |
| **MCP servers** | `.agents/mcp_config.json` | ⚠️ Reaches real external endpoints once given real tokens — configure per machine, never commit literal secrets |
| **Global (machine-local)** | `~/.gemini/config/` | Applies to every project on that machine; outside the repo by design |

## Priority

Highest to lowest: workspace `.agents/` → declared `skills.json` / `plugins.json` →
global `~/.gemini/config/` → built-in skills → global declared configs.

## ⚠️ Not `.antigravity/`

A `.antigravity/` folder is **not** read by Antigravity. Earlier versions of this repo
shipped one; it was inert and removed on 10-Sep-2026. If you are migrating from it, move
rules into the root `AGENTS.md` and skills into `.agents/skills/`.
