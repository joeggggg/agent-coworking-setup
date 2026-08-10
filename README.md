# Agent Co-Working Setup

Multi-agent coordination layer for **Claude Code**, **GitHub Copilot**, and **Antigravity (Gemini)** working the same repo. This is the *coordination* layer — entry point, persona, environment, spec-driven workflow, and the cross-agent handoff protocol. It does not contain domain skills (BA techniques, code review, etc.) — pair it with a skills repo such as [`ba-skills`](https://github.com/joeggggg/ba-skills).

## Why this exists

Three agents reading the same codebase without a shared contract step on each other: inconsistent tone, duplicated or conflicting rules, no way to hand a task from one agent to another mid-flight. This repo is that contract — one entry point (`AGENTS.md`), one persona, one environment doc, one delivery protocol, each agent's own isolated config directory underneath.

## How this composes with `ba-skills`

| Repo | Provides | Install target |
|---|---|---|
| `agent-coworking-setup` (this repo) | Entry point, persona, environment doc, spec-driven commands, cross-agent PRP protocol, per-agent config skeletons | Workspace/project root |
| [`ba-skills`](https://github.com/joeggggg/ba-skills) | `business-analyst` skill + `ba:` skill namespace (review, traceability, banking controls) | `.claude/skills/` (user- or project-level) |

Install this repo first (it defines the persona and conventions everything else follows), then layer `ba-skills` on top if the work is BA-flavored. If you install both at user level, skip `ba-skills`' own `CLAUDE.md` — this repo's `.claude/CLAUDE.md` is the superset; `ba-skills`' persona file is only meant for standalone BA-only installs where this repo isn't present.

## What's inside

```
agent-coworking-setup/
├── AGENTS.md                          # Universal entry point — every agent reads this first
├── CLAUDE.md                          # Environment doc template [CUSTOMIZE]
├── .claude/                           # Claude Code
│   ├── CLAUDE.md                      # Full persona — dual mode, output conventions [CUSTOMIZE]
│   ├── commands/                      # /explain /feature /init-spec-project /quality-gate
│   └── settings.template.json         # Plugins, MCP servers (env-var refs), model prefs
├── .antigravity/                      # Antigravity (Gemini)
│   ├── config.json
│   ├── rules.md                       # Code-modification & workflow standards
│   ├── ANTIGRAVITY.md                 # Same persona, rewired to point at .claude/
│   └── commands/                      # Pointer files mirroring .claude/commands
├── .github/                           # GitHub Copilot (real native config, not a placeholder)
│   ├── copilot-instructions.md
│   └── prompts/                       # /explain /feature /quality-gate as .prompt.md
├── templates/                         # Agent-neutral BA/product deliverable scaffolds
└── docs/
    └── prp-protocol.md                # Cross-Agent PRP Handoff Protocol
```

## Install

### Option A — Project-level (recommended for team use)

Copy everything except this README into your project root:

```powershell
Copy-Item -Recurse -Force "agent-coworking-setup\AGENTS.md","agent-coworking-setup\CLAUDE.md",`
  "agent-coworking-setup\.claude","agent-coworking-setup\.antigravity",`
  "agent-coworking-setup\.github","agent-coworking-setup\templates",`
  "agent-coworking-setup\docs" "your-project\"
```

### Option B — User-level (persona + commands available in every project)

```powershell
Copy-Item -Force ".claude\CLAUDE.md" "$env:USERPROFILE\.claude\CLAUDE.md"
Copy-Item -Recurse -Force ".claude\commands\*" "$env:USERPROFILE\.claude\commands\"
```

`AGENTS.md`, `.antigravity/`, `.github/`, `templates/`, and `docs/` are project-scoped by nature (they describe *this repo's* multi-agent contract) — copy those per-project, not user-level.

## Configuration

1. Open `.claude/CLAUDE.md` and `CLAUDE.md` and fill in every `[CUSTOMIZE]` field: name, role, domain, OS/shell, git user.
2. Open `.claude/settings.template.json`, delete any `mcpServers` entries you don't use, rename it to `settings.json`, and set the referenced env vars — never commit literal tokens.
3. Open `.antigravity/config.json` and confirm `project.name` and `project.domain` match your context.
4. If GitHub Copilot isn't in your toolchain, delete `.github/copilot-instructions.md` and `.github/prompts/` — nothing else depends on them.
5. If Antigravity isn't in your toolchain, delete `.antigravity/` — nothing else depends on it.

## The spec-driven commands

| Command | Does |
|---|---|
| `/init-spec-project` | Bootstraps a `docs/` spec suite (PRP, PRD, specs, delivery plan, system design, UI/UX, data dictionary, test plan, backlog, compliance) + a project `CLAUDE.md` |
| `/feature "<story>"` | Runs the full loop: ideate → plan (INVEST + Given/When/Then) → sync spec → RED → GREEN → review → gate → sync back |
| `/quality-gate` | Definition-of-Done check: lint, typecheck, tests, `any`-count, engine coverage, a11y, console errors |
| `/explain <topic>` | Structured explanation: Quick Overview → Main Content → Knowledge Reinforcement |

These are stack-agnostic — they auto-detect `package.json` scripts, `Makefile` targets, or ask.

## The cross-agent handoff protocol

For work that moves between agents (Claude Code interviews and authors a spec, Antigravity implements, Claude Code reviews against acceptance criteria), see [`docs/prp-protocol.md`](docs/prp-protocol.md). The PRP (Product Requirement Prompt) file is the single source of truth for the handoff; the filesystem is the evidence.

## Related

- [`ba-skills`](https://github.com/joeggggg/ba-skills) — companion skills repo for BA-flavored work
- [Claude Code Skills documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [GitHub Copilot custom instructions](https://docs.github.com/en/copilot/how-tos/custom-instructions) — native `.github/` config this repo's Copilot layer targets
