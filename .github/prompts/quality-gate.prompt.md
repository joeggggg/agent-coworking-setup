---
mode: agent
description: Definition-of-Done gate — lint, typecheck, tests, and manual DoD checks.
---

Enforce `docs/03-delivery-plan.md` §6 (Definition of Done). Do not call a feature "done" until this
is green.

1. Auto-detect and run the gate command in order: `npm run gate` → individual `lint`/`typecheck`/
   `test` scripts → `make gate` → Python (`ruff check . && mypy . && pytest`) → ask the user.
   Report the exact output of each command.
2. Verify non-script DoD items:
   - **Typing**: grep the diff for `any` / `as any` — target is 0 occurrences.
   - **Business logic coverage**: engine/core modules unit-tested; every formula in `docs/02-specs.md`
     §5 has a boundary test.
   - **Divide-by-zero safety**: every calculation returns 0 when the denominator is 0.
   - **Accessibility**: keyboard nav works; `prefers-reduced-motion` respected; no critical axe
     violations.
   - **Runtime health**: no console errors in the dev build; E2E smoke tests pass for UI changes.
3. Output a pass/fail checklist mapped to each DoD item, ending with an overall GATE: PASSED/FAILED
   line. If anything fails, stop and report the exact failure — do not mark the work complete.
