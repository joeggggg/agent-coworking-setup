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

## Multi-Machine & Sync Safety

Skip this section if the workspace lives on exactly one machine and is not inside a
cloud-synced folder. Otherwise it prevents a whole class of silent breakage.

### Path tokens — use these, never an absolute root

| Token | Resolves to | How to resolve |
|-------|-------------|----------------|
| `<WS>` | Workspace root | The directory containing `AGENTS.md` — resolve from the current working directory, never from a hardcoded path |
| `<LOCAL>` | Local code root, if the layout has one | Machine-local, **may be absent on the current machine** |
| `<USER>` | User-global config root | `$env:USERPROFILE` / `$HOME` — machine-local, never synced |

📌 Anything committed or synced must not name an absolute root. Write
`<WS>/docs/spec.md`, never `D:\Cloud\project\docs\spec.md`. Cloud clients let each
machine choose its own mount point, so a drive letter that is correct on one machine
is wrong on the next.

### ⚠️ There is only ONE sync boundary

If the workspace sits inside a cloud-synced folder (Google Drive, Dropbox, OneDrive,
iCloud), that client replicates **everything** beneath it. It does not read `.gitignore`.

> **`.gitignore` is not a sync boundary.** A file git ignores is still synced to every
> other machine.

This makes "machine-local file inside the synced workspace" a contradiction. In particular
`.claude/settings.json` is gitignored by this repo — that keeps secrets out of *git*, and
does nothing to stop a cloud client copying it, along with any absolute paths and
permission modes baked into it, onto every other machine.

| Config that is… | Belongs in | Never in |
|---|---|---|
| Portable (persona, conventions, commands, spec workflow) | `<WS>` — synced/committed | — |
| Machine-specific (permission modes, local tool paths, MCP servers, plugins) | `<USER>` — outside the synced folder | `<WS>`, even gitignored |

### Machine manifests

When a machine-specific fact must be *documented* inside `<WS>` — a mount point, a
toolchain version, what is installed where — put it in **one file per machine**:

```
setup/machines/<MACHINE-NAME>.md     # $env:COMPUTERNAME / $(hostname)
```

One file per machine means two machines never write the same file, so the cloud client
can never produce a conflict copy. A single shared inventory file is exactly what
produces them. Never edit another machine's manifest — you cannot see its disk.

### Version claims are machine-specific

`CLAUDE.md` records a runtime version (Node, Python, …). That claim is true of the machine
it was written on. Verify with `node -v` / `python --version` before relying on it, and
record per-machine values in the manifest above rather than editing the shared doc.

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
- Hardcode an absolute root or drive letter in anything synced or committed — use the
  path tokens in *Multi-Machine & Sync Safety*
- Put machine-specific config inside `<WS>`, even in a gitignored file — it still syncs
- `[CUSTOMIZE: any shell-chaining or platform-specific rule, e.g. PowerShell 5.1's lack of &&]`
