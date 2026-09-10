# Agent Co-Working Setup

Multi-agent coordination layer for **Claude Code**, **GitHub Copilot**, **Antigravity (Gemini)**, and **Cursor** working the same repo. Each agent's layer targets the path that agent genuinely reads — `.claude/`, `.github/`, `AGENTS.md` + `.agents/`, and `.cursor/` respectively. This is the *coordination* layer — entry point, persona, environment, spec-driven workflow, and the cross-agent handoff protocol. It does not contain domain skills (BA techniques, code review, etc.) — pair it with a skills repo such as [`ba-skills`](https://github.com/joeggggg/ba-skills).

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
│   └── settings.template.json         # Plugins, model prefs (MCP servers deliberately not included — see below)
├── .agents/                           # Antigravity (Gemini) — the path it actually reads
│   └── README.md                      # Skills/rules/plugins/hooks/MCP layout [OPTIONAL]
├── .cursor/                           # Cursor
│   ├── README.md                      # What Cursor actually reads, and what it doesn't
│   ├── rules/entry-point.mdc          # Always-on rule pointing at AGENTS.md
│   └── agents/implementer.md          # Subagent for spec-driven delegation [CUSTOMIZE]
├── .github/                           # GitHub Copilot (real native config, not a placeholder)
│   ├── copilot-instructions.md
│   └── prompts/                       # /explain /feature /quality-gate as .prompt.md
├── templates/                         # Agent-neutral BA/product deliverable scaffolds
├── setup/machines/                    # One manifest per machine [OPTIONAL — multi-machine setups]
└── docs/
    └── prp-protocol.md                # Cross-Agent PRP Handoff Protocol
```

## Install

### Option A — Project-level (recommended for team use)

Copy everything except this README into your project root:

```powershell
Copy-Item -Recurse -Force "agent-coworking-setup\AGENTS.md","agent-coworking-setup\CLAUDE.md",`
  "agent-coworking-setup\.claude","agent-coworking-setup\.agents",`
  "agent-coworking-setup\.cursor","agent-coworking-setup\.github",`
  "agent-coworking-setup\templates",`
  "agent-coworking-setup\docs" "your-project\"
```

### Option B — User-level (persona + commands available in every project)

```powershell
Copy-Item -Force ".claude\CLAUDE.md" "$env:USERPROFILE\.claude\CLAUDE.md"
Copy-Item -Recurse -Force ".claude\commands\*" "$env:USERPROFILE\.claude\commands\"
```

`AGENTS.md`, `.agents/`, `.cursor/`, `.github/`, `templates/`, and `docs/` are project-scoped by nature (they describe *this repo's* multi-agent contract) — copy those per-project, not user-level.

## Configuration

1. Open `.claude/CLAUDE.md` and `CLAUDE.md` and fill in every `[CUSTOMIZE]` field: name, role, domain, OS/shell, git user.
2. Open `.claude/settings.template.json`, rename it to `settings.json`. It ships without an `mcpServers` block on purpose — MCP servers (GitHub, Figma, Notion, etc.) reach real external endpoints once configured with a real token, so each new environment should get a deliberately-chosen, freshly-configured set rather than inheriting whatever a previous machine had. Add only what this environment actually needs, and never commit literal tokens — reference `${ENV_VARS}` set in your shell/profile.
3. Trim the `[CUSTOMIZE]` items in `AGENTS.md` § *Antigravity — agent-specific rules* (or delete the section if Antigravity isn't in your toolchain).
4. If GitHub Copilot isn't in your toolchain, delete `.github/copilot-instructions.md` and `.github/prompts/` — nothing else depends on them.
5. If Antigravity isn't in your toolchain, delete `.agents/` and the Antigravity section of `AGENTS.md` — nothing else depends on them.
6. If Cursor isn't in your toolchain, delete `.cursor/` — nothing else depends on it. If it is, fill in the `[CUSTOMIZE]` block in `.cursor/agents/implementer.md` with your stack's test and lint commands.
6. **If this workspace will be shared across machines**, read `AGENTS.md` §
   *Multi-Machine & Sync Safety* and keep `settings.json` in your **user** scope
   (`$env:USERPROFILE\.claude\`) rather than the project. See the warning below.

## The spec-driven commands

| Command | Does |
|---|---|
| `/init-spec-project` | Bootstraps a `docs/` spec suite (PRP, PRD, specs, delivery plan, system design, UI/UX, data dictionary, test plan, backlog, compliance) + a project `CLAUDE.md` |
| `/feature "<story>"` | Runs the full loop: ideate → plan (INVEST + Given/When/Then) → sync spec → RED → GREEN → review → gate → sync back |
| `/quality-gate` | Definition-of-Done check: lint, typecheck, tests, `any`-count, engine coverage, a11y, console errors |
| `/explain <topic>` | Structured explanation: Quick Overview → Main Content → Knowledge Reinforcement |

These are stack-agnostic — they auto-detect `package.json` scripts, `Makefile` targets, or ask.

## Delegating work between agents

The point of four config layers is that you can hand a task to whichever agent suits it and
have them all follow the same contract. A typical split:

| Stage | Agent | Why |
|---|---|---|
| Interview, scope, author the spec | Claude Code | Long-context reasoning; writes the PRP |
| Implement against the locked spec | Antigravity or Cursor | Cursor's `implementer` subagent runs in an isolated context so exploration doesn't pollute your main thread |
| Inline edits, quick refactors | Cursor | Fastest loop for in-editor work |
| Review delivered work vs acceptance criteria | Claude Code | Reports findings; does not silently fix |

Two rules make delegation work:

1. **The PRP is the contract, not the conversation.** Whatever the receiving agent needs must
   be in the file — it does not share your chat history. See [`docs/prp-protocol.md`](docs/prp-protocol.md).
2. **The receiving agent challenges gaps before building.** Silent reinterpretation is how a
   handoff produces the wrong thing convincingly.

Roles are defaults, not locks — any capable agent may take any role.

## The cross-agent handoff protocol

For work that moves between agents (Claude Code interviews and authors a spec, Antigravity implements, Claude Code reviews against acceptance criteria), see [`docs/prp-protocol.md`](docs/prp-protocol.md). The PRP (Product Requirement Prompt) file is the single source of truth for the handoff; the filesystem is the evidence.

## ⚠️ If your workspace lives in a cloud-synced folder

Putting the workspace inside Google Drive / Dropbox / OneDrive / iCloud is a fine way to
reach it from several machines, but it removes a boundary people assume they still have:

> **`.gitignore` is not a sync boundary.** The sync client replicates everything beneath
> the folder and never reads `.gitignore`. A gitignored `settings.json` — with its absolute
> paths, permission modes, and MCP endpoints — still lands on every one of your machines.

So there is no machine-local tier inside the workspace. Keep it this way instead:

| Config | Where | Why |
|---|---|---|
| Portable — persona, conventions, commands, spec workflow | Workspace (`<WS>`) | Same everywhere, by design |
| Machine-specific — permission modes, local tool paths, MCP servers, plugins | User scope (`<USER>`), outside the synced folder | Genuinely per-machine; divergence is correct |
| Machine facts you want *documented* in the repo | `setup/machines/<MACHINE-NAME>.md`, one file per machine | Two machines never write the same file, so the sync client can't create conflict copies |

And refer to roots by token (`<WS>`, `<LOCAL>`, `<USER>`) rather than by absolute path —
each machine may mount the same cloud folder under a different drive letter.

Full rules: `AGENTS.md` § *Multi-Machine & Sync Safety*.

## Related

- [`ba-skills`](https://github.com/joeggggg/ba-skills) — companion skills repo for BA-flavored work
- [Claude Code Skills documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [GitHub Copilot custom instructions](https://docs.github.com/en/copilot/how-tos/custom-instructions) — native `.github/` config this repo's Copilot layer targets
