---
description: Spec-driven feature loop — ideate, plan, sync specs, TDD, build, review, gate. Works in any project with docs/.
---

# /feature — Spec-driven feature loop

Run the full spec-driven loop for a feature or user story. `docs/` is the source of truth:
update specs BEFORE writing code, and keep them in sync as part of the change.

**Input:** `$ARGUMENTS` — a feature description or a story id (e.g. `US-08`, or "add user login").

## Pre-check

- Verify that `docs/` exists in this project. If not, tell the user:
  "This project doesn't have a `docs/` directory. Run `/init-spec-project` to set up the spec-driven workflow first."
- Read `CLAUDE.md` to understand the project context, tech stack, and conventions.

---

## Procedure

### 1. Ideate (scope check)
- Read `docs/00-product-requirement-prompt.md` goals + locked decisions.
- Confirm the request fits scope and the audience/tone/quality bar.
- If it conflicts with a locked decision (§3) or a RAID item (`docs/03` §5), surface the conflict
  and **ask** before proceeding.

### 2. Plan (decompose)
- Produce an **INVEST** user story with **Given/When/Then** acceptance criteria, in the exact style
  of `docs/03-delivery-plan.md` §2 "Sample story". Assign an epic, points, priority.
- Apply **Definition of Ready** (`docs/03` §6): AC present · referenced types exist in `docs/02` ·
  screen/token mapped in `docs/05`.

### 3. Sync spec (source of truth — do this BEFORE code)
- Add/extend the story in `docs/03-delivery-plan.md` (or `docs/08-backlog-and-wbs.md`).
- Add any new models/types/formulas to `docs/02-specs.md`; new data shapes to `docs/06-data-dictionary.md`.
- Add UI states to `docs/05-ui-ux-spec.md` if a screen changes.
- Register new test cases + AC↔test rows in `docs/07-test-plan.md`.

### 4. RED — failing tests first
- For each AC and each formula, write test cases (unit or component as appropriate).
- Map them back to the `docs/07` traceability table.
- Run the project's test command — confirm **RED** (tests fail because the code isn't written yet).
- Auto-detect test command: look for `test` script in `package.json`, or `Makefile`, or ask.

### 5. GREEN — implement
- Keep all business logic as **pure functions** in `src/engine/` (or equivalent logic layer).
- No `any`. Guard every division (return `0` when denominator is `0`).
- Implement UI in the appropriate component/screen directories.
- For visual/UX work, follow design tokens in `docs/02` §7 and patterns in `docs/05`.

### 6. REVIEW
- Review the diff for:
  - Strict typing — no `any`, no `as any`, sound unions.
  - Performance — no unnecessary re-renders, no unbounded loops.
  - Accessibility — keyboard nav, `prefers-reduced-motion`, semantic HTML.
- Address all findings before the gate.

### 7. GATE (Definition of Done)
- Run `/quality-gate`. Must be **green**.
- For UI-affecting changes, also run E2E smoke tests if available.

### 8. Sync back + commit
- Update `docs/07` traceability: mark test cases as passing.
- Mark the story status in `docs/03` or `docs/08`.
- Conventional commit message (`feat:`, `fix:`, `docs:`, `test:`, `refactor:`).
- Include a `docs: sync specs …` change when intent or specs changed.

---

## Output

Report when complete:
- **Story ID** and title
- **Files touched** (source + spec docs)
- **Specs updated** (which docs/ files changed)
- **Test cases added** (with AC↔test mapping)
- **Gate result** (pass/fail with details)
