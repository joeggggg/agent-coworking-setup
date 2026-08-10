---
description: Bootstrap a new project with the full spec-driven development workflow — docs/, CLAUDE.md, commands, hooks, and configs.
---

# /init-spec-project — Spec-Driven Project Initializer

Set up the complete spec-driven development workflow for a new or existing project.
`docs/` becomes the single source of truth. Code follows specs, not the other way around.

**Input:** `$ARGUMENTS` — project name, brief description, and tech stack.
Example: `"MyCoolApp" — a fitness tracking PWA — React + TypeScript + Vite + Tailwind`

If `$ARGUMENTS` is empty or incomplete, ask the user for:
1. **Project name** (human-readable)
2. **One-line description** (what does it do, who is it for)
3. **Tech stack** (framework, language, styling, state management, test framework)
4. **Test runner** (e.g. Vitest, Jest, Playwright — default: Vitest)
5. **Linter** (e.g. ESLint — default: ESLint)
6. **Build tool** (e.g. Vite, Webpack, Next.js — default: Vite)

Auto-detect defaults from existing `package.json` if present.

---

## Procedure

### Step 1 — Validate & Confirm

- Parse the project name, description, and stack from `$ARGUMENTS`.
- If the project already has a `docs/` directory, warn the user and ask whether to merge or overwrite.
- If a `CLAUDE.md` already exists, ask whether to replace or append.
- Confirm the plan with the user before writing any files.

### Step 2 — Create `docs/` spec documents

Create all 10 spec documents in `docs/`, each a **rich template** with correct heading structure,
placeholder guidance text, `TODO:` markers for sections the user must fill in, and example content:

| File | Role |
|---|---|
| `docs/00-product-requirement-prompt.md` | Build-ready PRP — role/mission, audience/tone, locked technical decisions, information architecture, core loop, must-have features, required models, core logic scaffolds, content guidelines, deliverables, quality bar |
| `docs/01-prd.md` | Executive summary, problem statement, goals/metrics, personas, high-level user stories, functional/non-functional requirements, out of scope, assumptions, open questions |
| `docs/02-specs.md` | Domain models (types), state machines/enums, business rules, API contracts, formulas & calculations, validation rules, design tokens |
| `docs/03-delivery-plan.md` | WBS, epic→story decomposition (INVEST + Given/When/Then sample), sequencing/dependencies, Gantt, RAID log, Definition of Ready/Done, what-to-build-first |
| `docs/04-system-design.md` | Architecture overview, component architecture, data flow, state management, folder structure, ADRs, security, performance |
| `docs/05-ui-ux-spec.md` | Screen inventory, screen specs, navigation/IA, design system, responsive breakpoints, motion, accessibility |
| `docs/06-data-dictionary.md` | Entity definitions, enumerations, relationships (ERD), storage format, validation rules |
| `docs/07-test-plan.md` | Test strategy by level, test case registry, AC↔test traceability, edge cases, coverage targets, conventions |
| `docs/08-backlog-and-wbs.md` | Product backlog, sprint/phase planning, grooming notes |
| `docs/09-legal-compliance-privacy.md` | Regulatory requirements, data privacy, analytics/tracking, terms, cookie/storage policy, age-related compliance if applicable |

Use `{{PROJECT_NAME}}`, `{{PROJECT_DESCRIPTION}}`, `{{TECH_STACK}}`, `{{TODAY}}`, `{{TEST_RUNNER}}` as
fill-in tokens while drafting, then replace them with actual values before writing the files.

### Step 3 — Generate `CLAUDE.md`

Create a project-root `CLAUDE.md` stating: what the project is; that `docs/` is the single source of
truth (never implement a feature whose intent/AC/types don't exist in `docs/` — update the spec
first); a table mapping each `docs/NN-*.md` file to its role; the feature loop (`/feature` — ideate →
plan → sync spec → RED → GREEN → review → gate → sync back); the Definition of Done; and conventions
(engine purity, no `any`, conventional commits, run/test/gate commands).

### Step 4 — Confirm `.claude/commands/feature.md` and `.claude/commands/quality-gate.md` are present

These ship with this repo (`agent-coworking-setup/.claude/commands/`) — copy them into the target
project if it doesn't already have them.

### Step 5 — Initialize git (if not already)

- Check if `.git/` exists. If not, run `git init`.
- Do NOT create a `.gitignore` if the project already has one.

### Step 6 — Summary

Print a summary of everything created, with file paths and next steps:
- "Run `/feature` to start your first story"
- "Edit `docs/00-product-requirement-prompt.md` first to define your product"

---

## Important Notes

- Replace all `{{...}}` tokens with actual values from user input before writing files.
- If a `package.json` exists, auto-detect: test runner, dev command, build command, lint command.
- The `docs/` templates should be written as **actual files**, not left as code blocks.
- Always ask before overwriting any existing file.
