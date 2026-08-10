# AGENTS.md — Universal Entry Point

Every AI agent (Claude Code, GitHub Copilot, Antigravity, or any future companion) reads this file first on workspace entry.

## Read Order

| Step | File | Purpose |
|------|------|---------|
| 1 | **`AGENTS.md`** (this file) | Who lives where, config boundaries, shared conventions |
| 2 | **`CLAUDE.md`** | Environment (OS, shell, tooling) and workspace layout |
| 3 | **`.claude/CLAUDE.md`** | Full persona, tone, slash commands, output conventions — shared by every agent, not just Claude Code |
| 4 | **Initiative/project `CLAUDE.md`** | Scoped context when working inside a sub-project — `[CUSTOMIZE: your initiative-folder convention, if any]` |
| 5 | **Your agent's config dir** | Tool-specific rules, commands, settings — **your directory only** |

Do not guess config locations. Do not read or modify another agent's config directory.

---

## Identity

`[CUSTOMIZE]`
- **Owner**: [name] — [role], [domain], [location/timezone]
- **Persona**: see `.claude/CLAUDE.md` for full tone, slash commands, and dual-mode rules

## Agent Configuration Directories

Each AI companion has an **isolated** configuration directory. Add commands, rules, or templates to **your own agent's directory only**. The owner manages cross-agent alignment manually.

| Agent | Config Dir | Key Files | Format |
|-------|------------|-----------|--------|
| **Claude Code** | `.claude/` | `CLAUDE.md` — persona, output conventions; `commands/` — slash commands; `settings.json` — plugins, MCP, permissions | Markdown + JSON |
| **GitHub Copilot** | `.github/` | `copilot-instructions.md` — repo-wide custom instructions; `prompts/*.prompt.md` — reusable prompts | Markdown |
| **Antigravity (Google DeepMind)** | `.antigravity/` | `ANTIGRAVITY.md` — main guidelines; `rules.md`; `config.json` | Markdown + JSON |

Both `.claude/CLAUDE.md` and `.antigravity/ANTIGRAVITY.md` carry the **same persona** — keep them in sync by hand when the persona changes. There is no automatic bridge between agent config directories in this setup.

### Skills

Domain skills (e.g. business-analysis techniques, code review checklists) are not part of this repo — install a skills package such as [`ba-skills`](https://github.com/joeggggg/ba-skills) into `.claude/skills/` separately. See this repo's `README.md` for how the two compose.

---

## Cross-Agent Delivery Protocol (PRP Handoff)

For complex tasks, work moves between agents through a **PRP** (Product Requirement Prompt) that lives alongside the project it describes. Full contract in [`docs/prp-protocol.md`](docs/prp-protocol.md).

Summary:

| Role | Default agent | Responsibility |
|------|---------------|-----------------|
| **Analyst / Spec author** | Claude Code | Interview the owner, clarify requirements, lock scope decisions, author the PRP |
| **Implementer** | Antigravity (Gemini) | Review the PRP, challenge gaps before building, implement, walk through the result |
| **Final reviewer** | Claude Code | Review delivered work against the PRP's acceptance criteria; report findings, don't silently fix |

Roles are defaults, not locks — any capable agent may take any role; the PRP contract is what matters.

---

## Default Workflow

1. Read `CLAUDE.md` for environment and directory layout
2. Read `.claude/CLAUDE.md` for persona and response style
3. If inside a scoped sub-project, read its `CLAUDE.md` for local context
4. For multi-step work → `/init-spec-project` (new project) or `/feature` (existing spec-driven project) — see `.claude/commands/`
5. For a spec-driven handoff between agents → author/consume a PRP per `docs/prp-protocol.md`
6. For diagrams → Mermaid.js

## Shared Output Conventions

Sourced from `.claude/CLAUDE.md` — do not diverge across agents:

- **Documents**: Markdown by default; other formats only when explicitly requested
- **Diagrams**: Mermaid.js (flowchart, sequence, ER, state, Gantt)
- **Specs**: User Story → Acceptance Criteria (Given/When/Then) → API/UI → Edge cases → Non-functional
- **Long tasks**: plan → confirm → execute — do not silently chain many steps
- **Tone**: direct, no fluff intros; lead with the answer

## Do Not

- Modify another agent's config directory
- Cross-contaminate rules between `.claude/`, `.antigravity/`, `.github/`
- Commit or push to git unless explicitly asked
- `[CUSTOMIZE: any shell-chaining or platform-specific rule, e.g. PowerShell 5.1's lack of &&]`
