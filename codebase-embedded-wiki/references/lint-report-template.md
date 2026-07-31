# Wiki Lint Report Template (`wiki-lint-report.md`)

Use this template to structure the output of a `lint` operation. The report must be saved as `wiki-lint-report.md` at the repository root or delivered as a structured output to the user.

---

## Template

```markdown
# Codebase Wiki Health Audit Report

- **Date:** 2026-07-31T17:30:00Z
- **Audited Commit:** `a1b2c3d4e5f6`
- **Total Wiki Pages:** 6

---

## Audit Summary
- 🟢 **Valid OKF Pages:** 6
- ⚠️ **Warnings:** 1
- 🔴 **Errors / Broken Links:** 0

---

## Findings

### 🔴 Errors (Action Required)
*None.*

### ⚠️ Warnings
1. **Stale Page**: `auth-service/wiki/concepts/jwt-strategy.md` (Stale date `2026-07-01` passed).
2. **Missing `wiki/index.md` TOC Entry**: `billing-service/wiki/index.md` is missing heading entries for `concepts/tax-calculation.md`.

### ℹ️ Recommendations
- Run `sync` operation to update stale page `auth-service/wiki/concepts/jwt-strategy.md`.
- Update `billing-service/wiki/index.md` down to 3 heading levels.
```
