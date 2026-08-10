# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation expands the codebase-embedded documentation from external sources — pull requests, commit ranges, or provided docs (RFCs, design docs, notes, postmortems) — distilling raw deltas into high-level *why/how* pages within affected module `docs/` directories.

---

## When to Run `ingest`

- Merging or reviewing a PR that introduces new concepts, architecture, or decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or a feature branch).
- Incorporating an external document (RFC, design doc, user notes).
- Onboarding an undocumented or partially documented repository.
- Generating architecture docs from the current codebase state.

`ingest` owns the whole pipeline, from bootstrap through distillation: a missing docs skeleton is a precondition handled **inside** `ingest` (Stage 0, below).

---

## Step-by-Step Procedure

### Stage 0: Bootstrap Setup — Scaffold Missing Skeletons (On Demand)

> The setup stage is a built-in first stage of `ingest`. For the full detail, see [setup-stage.md](setup-stage.md).

1. **Know the affected set first.** Parse the input (Step 1 below) to determine affected modules; when onboarding an undocumented repository, the whole repository is the affected set.
2. For each affected module, check for `README.md`, `docs/`, and `docs/index.md`.
3. **Bootstrap only the missing modules**, creating just the `docs/` branches where content will land — never the whole repo for a scoped PR, never pre-scaffolded `docs/<category>/` branches. Prefer `docs/` high in the tree: a parent module absorbs layered/stack submodules into one `README.md` + `docs/`.
4. Scaffold the **minimal skeleton** only: module `README.md` entry points and `docs/` + `docs/index.md` populated with real, known entries. No OKF pages or deep architecture content yet — those land in the content stages below.
5. **Preserve existing content** — never overwrite a `README.md`, `docs/index.md`, or page without approval. Confirm the scaffold with the user, batched alongside the new pages in Step 2.5.

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
5. **Watch page size**: if a page would exceed ~**500 lines** after this update, split it into focused pages instead of bloating the current one (see Step 2.5).

### Step 2.5: Confirm New Pages & Branches with the User

1. Present proposed additions (category, page title, module) before creating any new page or `docs/<category>/` directory.
2. **Confirm placement** — the right module and category. Ask for missing context if the input doesn't supply it.
3. Never create a directory that will end up empty.
4. **Batch, don't interleave**: present all proposed pages, branches, and ~500-line split proposals at once and apply after a single approval; only re-confirm if a new branch or category surfaces mid-run.

### Step 3: Write or Update OKF Pages & `docs/index.md` TOC

> See [okf-spec.md](okf-spec.md) and [index-templates.md](index-templates.md).

1. Format all new/updated pages with OKF YAML frontmatter:
   - `timestamp` = current ISO-8601 string; `type` = one of the [Canonical Types](okf-spec.md) — never introduce a value outside that list.
   - Update `tags`, add/update `related` (relative paths), set `resource`, and set `stale_after` when the rationale has a known expiry.
   - If the page being updated is past its `stale_after`, re-verify the documented rationale against its `resource` file(s) (or `git log`) before amending, and refresh `stale_after`/`timestamp` accordingly.
2. Synthesize clear, durable descriptions of high-level behavior.
3. **Update `<module>/docs/index.md` for affected modules only** — refresh the `index.md` TOC; leave unaffected indexes untouched.

### Step 4: Keep READMEs as Simple Entry Points

1. No OKF frontmatter in module `README.md`.
2. Verify affected module READMEs link to `docs/index.md`.
3. Don't modify READMEs of unaffected modules.

---

## Verification Criteria

- [ ] Only **affected modules** had files created, modified, or re-indexed.
- [ ] Missing skeletons were scaffolded in **Stage 0** only where content will land — no whole-repo bootstrap for a scoped PR, no pre-scaffolded branches.
- [ ] New pages and `docs/<category>/` branches were confirmed and created only where a page lands (no empty branches); directories opened **on demand**, not pre-scaffolded.
- [ ] Pages over ~**500 lines** were evaluated and any split was **proposed** before restructuring.
- [ ] All new/modified pages live strictly inside `docs/**/*.md`.
- [ ] Content reflects high-level *why/how*, not PR chatter or copied code.
- [ ] `docs/index.md` in affected dirs lists current pages as one-sentence bullets under their directory headings; unaffected indexes unchanged.
- [ ] `README.md` files have no OKF frontmatter and link to `docs/index.md`.
- [ ] Pages updated past `stale_after` were re-verified against `resource`/`git log` before amending, with `stale_after`/`timestamp` refreshed.
