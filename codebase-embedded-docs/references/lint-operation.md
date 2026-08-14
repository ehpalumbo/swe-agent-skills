# Operation: `lint` (Health-Check & Audit Documentation)

The `lint` operation health-checks the codebase-embedded documentation: ensures every `docs/` dir has an up-to-date `index.md` covering all its pages, audits OKF frontmatter, and flags orphan pages, duplication, and oversized pages.

---

## When to Run `lint`

- As part of CI or pre-commit checks.
- After bulk refactoring or updating many docs pages.
- Periodically to audit freshness, link health, and index completeness.

---

## Step-by-Step Procedure

### Step 1: Audit README Entry Point

Verify every module (and the root) README links to its `docs/index.md` — a missing or broken link is a **Missing Docs Index Link Error**.

### Step 2: Audit `docs/index.md` Completeness

For every `docs/` directory (root and module):

1. Verify `index.md` exists.
2. Verify headings mirror subdirectories (headings = directories, bullets = files).
3. Verify each heading carries a one-to-two-sentence scope description, written without a leading verb (per [index-templates.md](index-templates.md)) → violation.
4. Check every OKF file in `<module>/docs/**/*.md` is listed as a single-line, one-sentence bullet under its matching directory heading → missing or malformed. Summaries must synthesize page *content*, not restate its title or purpose label.

### Step 3: Validate OKF Frontmatter, Code Cloning & Page Size

For every Markdown file inside `docs/`:

1. Verify frontmatter starts and ends with `---`.
2. Check required OKF fields against [okf-spec.md](okf-spec.md).
3. **Audit code cloning**: large snippet blocks or line-by-line implementations → prefer high-level rationale.
4. **Flag oversized pages**: any page >~**500 lines** must be flagged as oversized with a concrete split proposal (focused pages it would break into, plus the `docs/index.md` update).

### Step 4: Validate File Links & Cross-References

1. Extract all markdown links (`[text](path)`) — from both page bodies and `related:` frontmatter entries.
2. Verify relative file targets exist on disk and are still consistent with docs reference context.
3. Flag absolute `file:///` URLs as non-portable links.
4. Flag missing targets as broken links.

### Step 5: Detect Orphan & Unindexed Pages

1. Collect all `<module>/docs/**/*.md` pages.
2. Verify each is indexed in `docs/index.md` or linked via `related`; flag unindexed/unlinked pages as orphan pages.

### Step 6: Generate the Docs Lint Report

Synthesize results into a structured report. Organize findings by severity. Include recommendations if applicable as action points.

## Verification Criteria

- [ ] `README.md` files with OKF frontmatter reported as Errors.
- [ ] README files missing or breaking their link to `docs/index.md` reported.
- [ ] Missing `docs/index.md` or index entries not listed under their directory headings reported as Errors/Warnings.
- [ ] Code snippet copying/cloning flagged as warnings.
- [ ] Docs pages >~500 lines flagged with an oversized Page Warning and split proposal.
- [ ] Broken file links reported as errors.
- [ ] Misplaced docs files outside `docs/` reported as errors.
- [ ] Frontmatter schema violations reported as errors.
- [ ] Non-portable URLs and orphan pages reported as warnings.
- [ ] Index headings without verb-less scope descriptions, or bullet summaries that restate page titles, reported as Warnings.
- [ ] `related:` frontmatter markdown links validated for broken targets.
- [ ] `resource:` frontmatter entries validated for missing file targets and reported as Warnings.
