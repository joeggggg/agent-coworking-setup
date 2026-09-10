# Antigravity AI Companion Guidelines

This document provides system and behavioral guidelines for the **Antigravity** (Google DeepMind) agent when working within this workspace.

---

## 💻 Environment & Tools

`[CUSTOMIZE — mirror the Environment section of the root CLAUDE.md so both files stay in sync]`

*   **Operating System**: [CUSTOMIZE]
*   **Shell**: [CUSTOMIZE] — note any command-chaining quirks
*   **Git**: Do not commit or push to Git unless explicitly asked.
*   **Multi-machine safety**: If this workspace is shared across machines or lives in a
    cloud-synced folder, read [AGENTS.md](file:///AGENTS.md) § *Multi-Machine & Sync Safety*
    before writing config. Never hardcode an absolute root or drive letter in anything
    committed or synced, and never place machine-specific settings inside the workspace —
    `.gitignore` does not stop a cloud client from syncing them.

### Antigravity Tool Guidelines
*   **`run_command`**: Execute shell commands with `Cwd` set to the relevant subdirectory of the workspace root.
*   **`view_file`**: Read files up to 800 lines at a time.
*   **`write_to_file`** / **`replace_file_content`** / **`multi_replace_file_content`**: Follow codebase conventions. Always preserve original document structure, commenting, and headers.
*   **`browser_subagent`**: Use when validation, manual UI tests, or browser automation is required.

---

## 👤 Persona (Shared with Claude Code)

Antigravity must adhere to the **same persona** established in `.claude/CLAUDE.md`. Do not restate it here — read that file. If the persona changes, update both files by hand; there is no automatic sync between agent config directories in this setup.

---

## 🤝 Cross-Companion Alignment

*   **Universal Entry Point**: Always read [AGENTS.md](file:///AGENTS.md) on entry to locate config directories and prevent cross-contamination.
*   **Environment & Layout**: Refer to [CLAUDE.md](file:///CLAUDE.md) at the root for environment settings and directory mapping.
*   **Persona & Conventions**: Consult [.claude/CLAUDE.md](file:///.claude/CLAUDE.md) before starting any task — it is the shared source of truth for tone, output conventions, and slash-command behavior.
*   **Templates**: Business/product deliverable scaffolds live in [templates/](file:///templates/) — agent-neutral, no Cursor or Claude-specific syntax.
*   **Slash Command Redirection**: When a pointer in [.antigravity/commands/](file:///.antigravity/commands/) is invoked, load and execute the matching instructions from [.claude/commands/](file:///.claude/commands/) or, if a skills package is installed, from `.claude/skills/`.
*   **Cross-Agent Handoff**: For work handed off between agents, follow [docs/prp-protocol.md](file:///docs/prp-protocol.md).
