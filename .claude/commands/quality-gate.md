---
description: Run the project's Definition-of-Done gate (lint + typecheck + tests) and report. Works in any project.
---

# /quality-gate — Definition of Done

Enforces `docs/03-delivery-plan.md` §6 (DoD). Do **not** call a feature "done" until this is green.

## Pre-check

- Verify that `docs/03-delivery-plan.md` exists. If not, warn that DoD items may be incomplete.
- Read `CLAUDE.md` for project-specific conventions and commands.

---

## Step 1 — Auto-detect and run the gate command

Try these in order:

1. **`package.json` → `gate` script** → run `npm run gate`
2. **`package.json` → individual scripts** → run `npm run lint && npm run typecheck && npm run test` (skip any that don't exist)
3. **`Makefile` → `gate` target** → run `make gate`
4. **`pyproject.toml` / `setup.cfg`** → run `ruff check . && mypy . && pytest`
5. **Fallback** — ask the user for the lint, typecheck, and test commands.

Report the **exact output** of each command.

---

## Step 2 — Verify non-script DoD items

After the automated gate passes, check these manually:

### Typing strictness
- `grep -rn "any\|as any" src/` — flag every occurrence of `any` or `as any` in the diff or codebase.
- Report count and locations. **0 `any`** is the target.

### Business logic coverage
- Engine/core logic modules are unit-tested.
- Boundary test exists for every formula in `docs/02-specs.md` §5.

### Divide-by-zero safety
- Every metric/calculation returns `0` when the denominator is `0`.
- Cross-reference with test plan edge cases.

### Accessibility
- Keyboard navigation works for interactive elements.
- `prefers-reduced-motion` is respected (animations disabled or reduced).
- No critical axe violations (if axe is configured).

### Runtime health
- No console errors in the dev build.
- For UI changes: E2E smoke tests pass (if configured).

---

## Output

A **pass/fail checklist** mapped to DoD items:

```
✅ Lint: passed
✅ Typecheck: passed (0 errors)
✅ Tests: 42 passed, 0 failed
✅ No `any`: 0 occurrences
✅ Engine coverage: formulas boundary-tested
✅ Divide-by-zero: all calculations guarded
✅ a11y: keyboard nav + reduced-motion
✅ No console errors
──────────────────────────────
GATE: ✅ PASSED
```

If **anything fails**, STOP — report what failed with the exact command output.
Do **not** mark the work complete.
