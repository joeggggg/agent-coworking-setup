# Cross-Agent PRP Handoff Protocol

For complex tasks, work moves between AI companions through a **PRP** (Product Requirement Prompt) that lives in the project folder it describes. The PRP is the single source of truth; the filesystem is the evidence.

## Roles

| Role | Default agent | Responsibility |
|------|---------------|-----------------|
| **Analyst / Spec author** | Claude Code | Interview the owner, clarify requirements, lock scope decisions, author the PRP |
| **Implementer** | Antigravity (Gemini) | Review the PRP, challenge gaps *before* building, implement, walk the owner through the result |
| **Final reviewer** | Claude Code | Review the delivered work against the PRP's acceptance criteria; report findings, don't silently fix |

Roles are defaults, not locks — any capable agent may take any role; the PRP contract is what matters.

## Stages

| # | Stage | Owner | Exit criteria |
|---|-------|-------|---------------|
| 1 | **Interview** | Analyst + owner | Ambiguities resolved via explicit questions; scope decisions recorded with the *why*; cost/effort implication stated next to any recommended option |
| 2 | **PRP authoring** | Analyst | PRP passes the cold-start test: an agent with zero conversation context can execute from it alone |
| 3 | **Implement** | Implementer | Reviews the PRP first and raises gaps/conflicts *before* building; executes the to-do in order; updates PRP §Status and §To-do as steps complete; ends with a walkthrough for the owner |
| 4 | **Walkthrough** | Implementer + owner | Owner has seen what was built, how, and what was deferred; feedback captured in the PRP |
| 5 | **Final review** (optional) | Reviewer | Work checked against PRP acceptance criteria; findings written to PRP §Review log (verdict + evidence per criterion); fixes only on the owner's instruction |

## PRP contract (required sections)

1. **Header** — status line, last-updated date, owner, pickup note
2. **Requirement** — what and why; locked scope decisions as a table, with rationale
3. **Deliverables & locations** — artifact/path/state table (absolute paths)
4. **Status** — ✅ done · 🔨 in progress (with exact blocker + known fix) · early findings
5. **To-do** — ordered steps with exact commands and paths
6. **Constraints / lessons** — binding rules for the continuing agent (incl. cost rules)
7. **Pickup instructions** — copy-paste block telling any agent exactly how to resume
8. **Acceptance criteria** — checkable list the final review grades against
9. **Review log** — appended by reviewers: date, agent, verdict per criterion, findings

## Rules

- **The PRP self-updates**: whichever agent works the task updates §Status/§To-do *before stopping*. A step isn't done until the PRP says so.
- **Disk beats document**: if the PRP and filesystem disagree, trust the filesystem and correct the PRP.
- **Implementer challenges, then commits**: gaps found in the PRP are raised as questions or logged as assumptions in the PRP — never silently reinterpreted.
- **Reviewer reports, doesn't rewrite**: final review produces findings against acceptance criteria; changes happen only on the owner's instruction.

## Where the PRP lives

`[CUSTOMIZE — e.g. <project-root>/PRP.md, or a docs/ subfolder if using the /init-spec-project suite, where it would sit alongside docs/00-product-requirement-prompt.md as the cross-agent handoff variant of that same document]`
