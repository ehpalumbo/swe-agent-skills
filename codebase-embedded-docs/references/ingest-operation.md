# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation expands the codebase-embedded documentation from external sources — pull requests, commit ranges, or provided docs (RFCs, design docs, notes, postmortems) — distilling raw deltas into high-level *why/how* pages within affected module `docs/` directories.

---

## When to Run `ingest`

- Merging or reviewing a PR that introduces new concepts, architecture, or decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or a feature branch).
- Incorporating an external document (RFC, design doc, user notes).
- Onboarding an undocumented or partially documented repository.
- Generating architecture docs from the current codebase state.

`ingest` owns the whole pipeline, from storage planning through distillation: deciding **where** each new piece of documentation lives in the docs tree is a built-in step (Step 2, below), not a separate one-time bootstrap.

---

## Step-by-Step Procedure

### Step 1: Parse the Input Source & Identify Affected Modules

1. Determine the source type:
   - **Pull Request / Commit Range:** extract modified files, diffs, PR description, and commit messages via `git log` / `git diff`.
   - **External Document / RFC / Design Doc:** read the provided file or text.
   - **Current Codebase State:** generate architecture docs from the current state.
2. Identify the affected modules from file paths or domain topics. When onboarding an undocumented repository, the whole repository is the affected set.
3. **Keep scope limited to these modules** — untouched modules stay untouched.
4. Note each affected module's `README.md` / `docs/` / `docs/index.md` presence — Step 2 plans storage against it.

### Step 2: Set Up Storage — Plan Where Each Piece Will Live (Read-Only)

1. **Infer `docs/` placement — prefer higher-level branches.**
   - Check the root for an existing `README.md` and `docs/`.
   - Infer modules from the directory layout and manifests (`package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, `pyproject.toml`). Don't exhaustively scan large trees.
   - **Identify logical components, not module folders.** Walking top-down, decide whether each submodule is a real component needing docs, or just a layer/submodule whose docs belong to its parent.
   - **Consolidate `docs/` at parent modules.** When several submodules are layers of one application stack, propose a single parent `docs/` (`+ README.md`) whose `index.md` sections enumerate the sub-layers — not a `docs/` per layer.
   - Produce a proposed list of **affected branches** (root + each selected `docs/` host), noting each entry's `README.md` / `docs/` presence and which child submodules a parent covers.
2. **Plan the storage per host.** Review each affected host's existing pages and categories (`<module>/docs/**/*.md`, `<module>/docs/index.md`). **Start small** — only open `docs/<category>/` branches that will hold content. The per-piece decision (extend an existing page vs. open a new page vs. open a new branch) is finalized during distillation in Step 3, always preferring the smallest available fit. **Watch page size** there too: never wire a ~**500-line** page for another update when a split into focused pages fits better.
3. **Plan a minimal skeleton for affected modules missing `README.md` + `docs/` + `docs/index.md`:**
   - `README.md` entry points **only for affected modules**; a parent absorbing child layers gets **one** parent `README.md` describing the whole component and linking to `docs/index.md` — leaf-layer submodules get none of their own.
   - **One `docs/` + `docs/index.md`**, populated with real, known entries — no fabricated placeholder headings. Where submodules are stack layers, use **one** parent `docs/index.md` to enumerate the sub-layers rather than nesting child `docs/` trees.
   - `docs/<category>/` subdirectories **only where pages will be placed** — never a category branch just because it's listed, never a directory that will end up empty.
   - Infer what you can from manifests, READMEs, and source layout; **ask the user when context is insufficient** (e.g., "What does `<module>` do and what are its boundaries?"). Collect high-level facts only.
   - No OKF pages or deep architecture content — those land in the content steps below. Never bootstrap the whole repo for a scoped PR, never pre-scaffold `docs/<category>/` branches.
4. **Preserve existing content** — never overwrite a `README.md`, `docs/index.md`, or page without approval.

> **Scope to where docs will actually live.** Lean toward fewer, higher-level `docs/` roots. Never scaffold a `docs/` for a leaf layer that fits under its parent, nor for empty/placeholder modules.

### Step 3: Map Insights to Storage Placements & High-Level Topics

For each **affected** module:

1. Locate its `README.md` and `docs/`.
2. Review existing pages in `<module>/docs/**/*.md` and `<module>/docs/index.md`.
3. Distill input into high-level *why/how* concepts. **Don't copy code snippets, commit messages, or diffs**.
4. Decide whether to:
   - **Update an existing OKF page** (add insights, update `timestamp`), or
   - **Create a new OKF page** at `<module>/docs/<category>/<topic-slug>.md`. Prefer the smallest fit — reuse existing structure, and open a new `docs/<category>/` directory only when the module lacks a matching one and the content justifies it.
5. **Watch page size**: if a page would exceed ~**500 lines** after this update, split it into focused pages instead of bloating the current one (proposed in Step 4).

### Step 4: Present the Full Storage Plan & New Pages for One Sign-off

1. Present the complete plan in one batch: the storage placements and scaffold from Step 2 **plus** the proposed additions (module, page title, category) and ~500-line split proposals from Step 3.
2. **Confirm placement** — the right module, docs host, and category; confirm which submodules are consolidated under a parent. Ask for missing context if the input doesn't supply it.
3. Never create a directory that will end up empty.
4. **Batch, don't interleave**: apply after a single approval; only re-confirm if a new branch or category surfaces mid-run. For unattended runs, one sign-off for the whole batch.

### Step 5: Create Confirmed Scaffold & Write/Update OKF Pages + `docs/index.md` TOC

1. Create the confirmed minimal skeletons for modules scaffolded in Step 2: `docs/` directories with `docs/index.md` populated with real entries, and only the `docs/<category>/` branches that hold pages.
2. Format all new/updated pages with OKF YAML frontmatter:
   - `timestamp` = current ISO-8601 string; `type` = one of the [Canonical Types](okf-spec.md) — never introduce a value outside that list.
   - Update `tags`, add/update `related` (relative paths), set `resource`, and set `stale_after` when the rationale has a known expiry.
   - If the page being updated is past its `stale_after`, re-verify the documented rationale against its `resource` file(s) (or `git log`) before amending, and refresh `stale_after`/`timestamp` accordingly.
3. Synthesize clear, durable descriptions of high-level behavior.
4. **Update `<module>/docs/index.md` for affected modules only** — refresh the `index.md` TOC; leave unaffected indexes untouched.

> See [okf-spec.md](okf-spec.md), [index-templates.md](index-templates.md), and [readme-template.md](readme-template.md).

### Step 6: Keep READMEs as Simple Entry Points

1. No OKF frontmatter in module `README.md`.
2. Verify affected module READMEs link to `docs/index.md`.
3. Don't modify READMEs of unaffected modules.

---

## Verification Criteria

- [ ] Only **affected modules** had files created, modified, or re-indexed.
- [ ] Storage placement was planned **read-only** for **every** ingest — no files created until the Step 4 sign-off.
- [ ] `docs/` roots were **consolidated high in the tree**: parent modules with layer submodules hold one `README.md` + `docs/` covering their children; leaf layers have no redundant `docs/`.
- [ ] Missing skeletons planned in Step 2 **only where content will land** — no whole-repo bootstrap for a scoped PR, no pre-scaffolded `docs/<category>/` branches.
- [ ] Every planned `docs/index.md` reflects real context (not fabrications) and lists only branches that hold pages.
- [ ] The storage plan and new pages/`docs/<category>/` branches were confirmed in **one batch sign-off** (Step 4); directories opened **on demand**, only where a page lands (no empty branches).
- [ ] Pages over ~**500 lines** were evaluated and any split was **proposed** before restructuring.
- [ ] All new/modified pages live strictly inside `docs/**/*.md`.
- [ ] Content reflects high-level *why/how*, not PR chatter or copied code.
- [ ] `docs/index.md` in affected dirs lists current pages as one-sentence bullets under their directory headings; every affected `docs/` directory contains an `index.md`; unaffected indexes unchanged.
- [ ] `README.md` files have no OKF frontmatter and link to `docs/index.md`.
- [ ] Pages updated past `stale_after` were re-verified against `resource`/`git log` before amending, with `stale_after`/`timestamp` refreshed.
