---
mode: agent
description: Spec-driven feature loop — ideate, plan, sync specs, TDD, build, review, gate.
---

Run the full spec-driven loop for ${input:feature}. `docs/` is the source of truth: update specs
BEFORE writing code, and keep them in sync as part of the change.

Pre-check: verify `docs/` exists (if not, tell the user to run `/init-spec-project` first). Read
`CLAUDE.md` for project context and conventions.

1. **Ideate** — read `docs/00-product-requirement-prompt.md` goals + locked decisions; confirm scope
   fit; surface and ask about any conflict with a locked decision or RAID item before proceeding.
2. **Plan** — produce an INVEST user story with Given/When/Then acceptance criteria in the style of
   `docs/03-delivery-plan.md` §2; apply Definition of Ready.
3. **Sync spec first** — extend `docs/03` (or `08`), add models/formulas to `docs/02`, data shapes to
   `docs/06`, UI states to `docs/05`, test cases to `docs/07` — before any code.
4. **RED** — write failing tests mapped to the `docs/07` traceability table; confirm they fail.
5. **GREEN** — implement; business logic as pure functions; no `any`; guard every division.
6. **Review** — strict typing, performance, accessibility.
7. **Gate** — run the `/quality-gate` prompt; must be green.
8. **Sync back** — update `docs/07` traceability and story status; conventional commit including
   `docs: sync specs ...` when intent changed.

Report: story ID, files touched, specs updated, test cases added (with AC mapping), gate result.
