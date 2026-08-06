# Operation: `lint` (Health-Check & Audit Documentation)

The `lint` operation health-checks the codebase-embedded documentation: verifies READMEs aren't OKF, ensures every `docs/` dir has an up-to-date `index.md` covering all its pages, audits OKF frontmatter, and flags orphan/stale pages, code duplication, and oversized pages.

---

## When to Run `lint`

- As part of CI or pre-commit checks.
- After bulk refactoring or updating many docs pages.
- Periodically to audit freshness, link health, and index completeness.

---

## Step-by-Step Procedure

### Step 1: Audit README Decoupling

1. Verify **no `README.md` contains OKF YAML frontmatter** — if any does, flag an **Invalid README Format Error**.
2. Verify each module README links to `docs/index.md`.

### Step 2: Audit `docs/index.md` Completeness

For every `docs/` directory (root and module):

1. Verify `index.md` exists — missing → **Missing Docs Index Error**.
2. Verify headings mirror subdirectories (headings = directories, bullets = files).
3. Verify each heading carries a short scope description of its directory.
4. Check every OKF file in `<module>/docs/**/*.md` is listed as a single-line, one-sentence bullet under its matching directory heading → missing or malformed → **Incomplete Index Warning**.

### Step 3: Validate OKF Frontmatter, Code Cloning & Page Size

For every Markdown file inside `docs/`:

1. Verify frontmatter starts and ends with `---`.
2. Check required OKF fields against [okf-spec.md](okf-spec.md).
3. **Audit code cloning**: large snippet blocks or line-by-line implementations → **Code Cloning Warning** (prefer high-level rationale).
4. **Flag oversized pages**: any page >~**500 lines** → **Oversized Page Warning** with a concrete split proposal (focused pages it would break into, plus the `docs/index.md` update).

### Step 4: Validate File Links & Cross-References

1. Extract all links (`[text](path)`).
2. Verify relative file targets exist on disk.
3. Flag absolute `file:///` URLs as **Non-Portable Link Warnings**.
4. Flag missing targets as **Broken Link Errors**.

### Step 5: Detect Orphan & Unindexed Pages

1. Collect all `<module>/docs/**/*.md` pages.
2. Verify each is indexed in `docs/index.md` or linked via `related`; flag unindexed/unlinked pages as **Orphan Page Warnings**.

### Step 6: Generate the Docs Lint Report

Synthesize results into a structured report:

```markdown
# Codebase Docs Health Audit Report

## 🔴 Errors (Action Required)
*None.*

## ⚠️ Warnings
- **Missing `docs/index.md` TOC Entry**: `billing-service/docs/index.md` is missing the bullet entry for `concepts/tax-calculation.md` under its `## Concepts` heading.
- **Oversized Page (>500 lines)**: `billing-service/docs/concepts/tax-calculation.md` is 640 lines. Propose splitting into `tax-calculation.md`, `tax-rates.md`, and `tax-reporting.md`.

## ℹ️ Recommendations
- Update `billing-service/docs/index.md` to list all pages as one-sentence bullets under their directory headings.
```

---

## Verification Criteria

- [ ] `README.md` files with OKF frontmatter reported as Errors.
- [ ] Missing `docs/index.md` or index entries not listed under their directory headings reported as Errors/Warnings.
- [ ] Code snippet copying/cloning flagged as Warnings.
- [ ] Docs pages >~500 lines flagged with an Oversized Page Warning and split proposal.
- [ ] Broken file links reported as Errors.
- [ ] Misplaced docs files outside `docs/` reported as Errors.
- [ ] Frontmatter schema violations reported as Errors.
- [ ] Non-portable URLs and orphan pages reported as Warnings.
