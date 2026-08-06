# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation expands the codebase-embedded documentation from external sources — pull requests, commit ranges, or provided docs (RFCs, design docs, notes, postmortems) — distilling raw deltas into high-level *why/how* pages within affected module `docs/` directories.

---

## When to Run `ingest`

- Merging or reviewing a PR that introduces new concepts, architecture, or decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or a feature branch).
- Incorporating an external document (RFC, design doc, user notes).
- Generating architecture docs from the current codebase state.

---

## Step-by-Step Procedure

### Precondition: Bootstrap Docs Skeleton for Affected Modules Only

If an affected module isn't bootstrapped, run the [init operation](init-operation.md) **scoped to that module only**, creating just the `docs/` branches where content will land. Don't bootstrap the whole repo.

### Step 1: Parse the Input Source & Identify Affected Modules

1. Determine the source type:
   - **Pull Request / Commit Range:** extract modified files, diffs, PR description, and commit messages via `git log` / `git diff`.
   - **External Document / RFC / Design Doc:** read the provided file or text.
   - **Current Codebase State:** generate architecture docs from the current state.
2. Identify the affected modules from file paths or domain topics.
3. **Keep scope limited to these modules** — untouched modules stay untouched.

### Step 2: Map Insights to Modules & High-Level Topics

For each **affected** module:

1. Locate its `README.md` and `docs/`.
2. Review existing pages in `<module>/docs/**/*.md` and `<module>/docs/index.md`.
3. Distill input into high-level *why/how* concepts. **Don't copy code snippets, commit messages, or diffs**.
4. Decide whether to:
   - **Update an existing OKF page** (add insights, update `timestamp`), or
   - **Create a new OKF page** at `<module>/docs/<category>/<topic-slug>.md`. Prefer the smallest fit — reuse existing structure, and open a new `docs/<category>/` directory only when the module lacks a matching one and the content justifies it.
5. **Watch page size**: if a page would exceed ~**500 lines** after this update, consider placing the new content in a dedicated page instead of bloating the current one.

### Step 2.5: Confirm New Pages & Branches with the User

1. Present proposed additions (category, page title, module) before creating any new page or `docs/<category>/` directory.
2. **Confirm placement** — the right module and category. Ask for missing context if the input doesn't supply it.
3. Never create a directory that will end up empty.
4. When a page would exceed ~**500 lines**, **propose splitting** it into focused pages and confirm before restructuring. Propose; don't silently split.

### Step 3: Write or Update OKF Pages & `docs/index.md` TOC

> See [okf-spec.md](okf-spec.md) and [index-templates.md](index-templates.md).

1. Format all new/updated pages with OKF YAML frontmatter:
   - `timestamp` = current ISO-8601 string; `type` in (`architecture`, `decision-record`, `concept`, `guide`).
   - Update `tags`, add/update `related` (relative paths), set `resource`, and set `stale_after` when the rationale has a known expiry.
2. Synthesize clear, durable descriptions of high-level behavior.
3. **Update `<module>/docs/index.md` for affected modules only** — refresh the `index.md` TOC; leave unaffected indexes untouched.

### Step 4: Keep READMEs as Simple Entry Points

1. No OKF frontmatter in module `README.md`.
2. Verify affected module READMEs link to `docs/index.md`.
3. Don't modify READMEs of unaffected modules.

---

## Verification Criteria

- [ ] Only **affected modules** had files created, modified, or re-indexed.
- [ ] New pages and `docs/<category>/` branches were confirmed and created only where a page lands (no empty branches); directories opened **on demand**, not pre-scaffolded.
- [ ] Pages over ~**500 lines** were evaluated and any split was **proposed** before restructuring.
- [ ] All new/modified pages live strictly inside `docs/**/*.md`.
- [ ] Content reflects high-level *why/how*, not PR chatter or copied code.
- [ ] `docs/index.md` in affected dirs lists current pages as one-sentence bullets under their directory headings; unaffected indexes unchanged.
- [ ] `README.md` files have no OKF frontmatter and link to `docs/index.md`.
