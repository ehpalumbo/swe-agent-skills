# Operation: `lint` (Health-Check & Audit Documentation)

The `lint` operation performs a comprehensive health check on the codebase-embedded documentation. It verifies that `README.md` files do not follow OKF format, ensures every `docs/` directory contains an up-to-date `index.md` Table of Contents indexed down to 3 levels deep, audits OKF YAML frontmatter compliance, flags orphan or stale pages (`stale_after`) and checks for code duplication/cloning.

---

## When to Run `lint`

- As part of continuous integration (CI) or pre-commit checks.
- After bulk refactoring or updating multiple docs pages.
- Periodically to audit documentation freshness, link health, and index completeness.

---

## Detailed Step-by-Step Procedure

### Step 1: Audit Manifest & README Decoupling

1. Scan repository modules and verify that **NO `README.md` file contains OKF YAML frontmatter**. If any `README.md` has OKF frontmatter, flag as an **Invalid README Format Error**.
2. Verify that every module `README.md` contains a valid relative link to `docs/index.md`.

### Step 2: Audit `docs/index.md` Completeness (3 Levels Deep)

For every `docs/` directory (root `docs/` and module `<module>/docs/` folders):

1. Verify that `index.md` exists. If missing, report a **Missing Docs Index Error**.
2. Read `index.md` and verify it contains a Table of Contents indexed **up to 3 levels deep** (`# Title`, `## Section`, `### Sub-section`).
3. Check that every OKF Markdown file in `<module>/docs/**/*.md` (including subdirectories reflecting upper docs levels) is listed in `index.md`. If a page or heading is missing, report an **Incomplete Index Warning**.

### Step 3: Validate OKF YAML Frontmatter & Code Cloning Rules

For every Markdown file inside `docs/` subdirectories:

1. Verify frontmatter starts with `---` and ends with `---`.
2. Check required OKF v0.2 fields against defined fields in [OKF format](okf-spec.md).
3. **Audit for Code Cloning**: Inspect page body for large code snippet blocks or line-by-line code implementations. If present, report a **Code Cloning Warning** (encourage high-level rationale over code repetition).

### Step 4: Validate File Links & Markdown Cross-References

For every Markdown docs page:

1. Extract all Markdown links (`[text](path)`).
2. Check relative file links: Verify that target Markdown files or source files exist on disk relative to the page path.
3. Flag any absolute `file:///` URLs as **Non-Portable Link Warnings** (encourage relative paths).
4. Flag any missing targets as **Broken Link Errors**.

### Step 5: Detect Orphan & Unindexed Pages

1. Collect all `<module>/docs/**/*.md` pages across all modules.
2. Verify every page is indexed in the local `docs/index.md` or linked via `related` frontmatter.
3. Flag any unindexed/unlinked pages as **Orphan Page Warnings**.

### Step 6: Generate Docs Lint Report

Synthesize audit results into a structured Docs Lint Report following this template:

```markdown
# Codebase Docs Health Audit Report

## 🔴 Errors (Action Required)
*None.*

## ⚠️ Warnings
- **Missing `docs/index.md` TOC Entry**: `billing-service/docs/index.md` is missing heading entries for `concepts/tax-calculation.md`.

## ℹ️ Recommendations
- Update `billing-service/docs/index.md` to cover up to 3 levels deep.
```

---

## Verification Criteria for `lint`

- [ ] `README.md` files with OKF frontmatter are reported as Errors.
- [ ] Missing `docs/index.md` files or incomplete 3-level TOCs are reported as Errors/Warnings.
- [ ] Code snippet copying/cloning in docs pages is flagged as Warnings.
- [ ] All broken file links are identified and reported as Errors.
- [ ] Misplaced docs files outside `docs/` directories are reported as Errors.
- [ ] Frontmatter schema violations are reported as Errors.
- [ ] Non-portable absolute URLs and orphan pages are reported as Warnings.
