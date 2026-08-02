# Operation: `lint` (Health-Check & Audit Wiki)

The `lint` operation performs a comprehensive health check on the codebase-embedded wiki. It verifies that `README.md` files do not follow OKF format, ensures every `wiki/` directory contains an up-to-date `index.md` Table of Contents indexed down to 3 levels deep, audits OKF YAML frontmatter compliance, flags orphan or stale pages (`stale_after`), checks for code duplication/cloning, and verifies that `.wiki-meta.json` tracking SHA is up to date with `git HEAD`.

---

## When to Run `lint`

- As part of continuous integration (CI) or pre-commit checks.
- After bulk refactoring or updating multiple wiki pages.
- Periodically to audit documentation freshness, link health, and index completeness.

---

## Detailed Step-by-Step Procedure

### Step 1: Audit Manifest & README Decoupling

1. Check if `.wiki-meta.json` exists at the repository root and contains valid minimal JSON (`last_sync_commit`, `last_sync_timestamp`).
2. Read `last_sync_commit` from `.wiki-meta.json` and compare it against `git rev-parse HEAD`. If different, report a **Sync Lag Warning**.
3. Scan repository modules and verify that **NO `README.md` file contains OKF YAML frontmatter**. If any `README.md` has OKF frontmatter, flag as an **Invalid README Format Error**.
4. Verify that every module `README.md` contains a valid relative link to `wiki/index.md`.

### Step 2: Audit `wiki/index.md` Completeness (3 Levels Deep)

For every `wiki/` directory (root `wiki/` and module `<module>/wiki/` folders):

1. Verify that `index.md` exists. If missing, report a **Missing Wiki Index Error**.
2. Read `index.md` and verify it contains a Table of Contents indexed **up to 3 levels deep** (`# Title`, `## Section`, `### Sub-section`).
3. Check that every OKF Markdown file in `<module>/wiki/**/*.md` (including subdirectories reflecting upper wiki levels) is listed in `index.md`. If a page or heading is missing, report an **Incomplete Index Warning**.

### Step 3: Validate OKF YAML Frontmatter & Code Cloning Rules

For every Markdown file inside `wiki/` subdirectories:

1. Verify frontmatter starts with `---` and ends with `---`.
2. Check required OKF v0.2 fields:
   - `type`: Must be one of `architecture`, `decision-record`, `concept`, `guide`.
   - `title`: Non-empty string.
   - `description`: Non-empty string focusing on high-level rationale.
   - `module`: Valid module path or `root`.
   - `tags`: Non-empty array of strings.
   - `timestamp`: Valid ISO-8601 string.
3. **Audit for Code Cloning**: Inspect page body for large code snippet blocks or line-by-line code implementations. If present, report a **Code Cloning Warning** (encourage high-level rationale over code repetition).
4. If `stale_after` is present, compare against current timestamp. If current date > `stale_after`, flag as **Stale Page Warning**.

### Step 4: Validate File Links & Markdown Cross-References

For every Markdown wiki page:

1. Extract all Markdown links (`[text](path)`).
2. Check relative file links: Verify that target Markdown files or source files exist on disk relative to the page path.
3. Flag any absolute `file:///` URLs as **Non-Portable Link Warnings** (encourage relative paths).
4. Flag any missing targets as **Broken Link Errors**.

### Step 5: Detect Orphan & Unindexed Pages

1. Collect all `<module>/wiki/**/*.md` pages across all modules.
2. Verify every page is indexed in the local `wiki/index.md` or linked via `related` frontmatter.
3. Flag any unindexed/unlinked pages as **Orphan Page Warnings**.

### Step 6: Generate Wiki Lint Report

Synthesize audit results into a structured Wiki Lint Report (following the template in [lint-report-template.md](lint-report-template.md)).

---

## Verification Criteria for `lint`

- [ ] `README.md` files with OKF frontmatter are reported as Errors.
- [ ] Missing `wiki/index.md` files or incomplete 3-level TOCs are reported as Errors/Warnings.
- [ ] Code snippet copying/cloning in wiki pages is flagged as Warnings.
- [ ] All broken file links are identified and reported as Errors.
- [ ] Misplaced wiki files outside `wiki/` directories are reported as Errors.
- [ ] Frontmatter schema violations are reported as Errors.
- [ ] Non-portable absolute URLs, stale pages (`stale_after`), and orphan pages are reported as Warnings.
- [ ] Out-of-sync manifest commit is flagged with clear instructions to run `sync`.
