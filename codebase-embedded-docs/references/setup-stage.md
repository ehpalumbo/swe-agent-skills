# Setup Stage: Bootstrap the Docs Skeleton (Stage 0 of `ingest`)

This is the deep-dive for **Stage 0** of the [`ingest`](ingest-operation.md) operation. A docs skeleton is a precondition for content generation, so bootstrapping runs first, before any knowledge is distilled.

The setup stage scaffolds doc structure minimalistically — **no** full scans, wholesale generation, OKF pages, or deep architecture content. It only creates the doc roots needed to begin ingesting: `README.md` entry points, `docs/` directories, and `docs/index.md` indexes.

**Start small and grow on demand**: new `docs/<category>/` branches appear only when content justifies them.

**Docs live high in the tree** — model `docs/` placement on the logical component, not the physical folder of every module; a parent module often owns the docs for its children (e.g., a `billing-service` parent whose `api/`, `core/`, `persistence/`, `workers/` are functional layers). One parent `docs/` beats many thin leaf `docs/`.

---

## When the Setup Stage Applies

- First-time docs on a repository (runs as a **codebase-state** `ingest`).
- Onboarding an undocumented or partially documented repository.
- Re-bootstrapping after major restructuring.
- Any `ingest` whose affected module lacks `README.md` + `docs/` + `docs/index.md`.

It does **not** apply when folding PRs, commit ranges, or external docs into already-bootstrapped modules — skip to [`ingest`](ingest-operation.md) Step 2 directly.

---

## Step-by-Step Procedure

### Step 1: Infer `docs/` Placement — Prefer Higher-Level Branches (Read-Only)

1. Check the root for an existing `README.md` and `docs/`.
2. Infer modules from the directory layout and manifests (`package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, `pyproject.toml`). Don't exhaustively scan large trees.
3. **Identify logical components, not module folders.** Walking top-down, decide each submodule is a real component needing docs, or just a layer/submodule whose docs belong to its parent.
4. **Consolidate `docs/` at parent modules.** When several submodules are layers of one application stack, propose a single parent `docs/` (`+ README.md`) whose `index.md` sections enumerate the sub-layers — not a `docs/` per layer.
5. Produce a proposed list of **affected branches** (root + each selected `docs/` host), noting each entry's `README.md` / `docs/` presence and which child submodules a parent covers.

> **Scope to where docs will actually live.** Lean toward fewer, higher-level `docs/` roots. Never scaffold a `docs/` for a leaf layer that fits under its parent, nor for empty/placeholder modules. Keep this step **read-only**.

### Step 2: Confirm the Inferred Module Structure with the User

1. Present the affected branches and the planned scaffold for each (`README.md`, `docs/`, `docs/index.md`).
2. **Confirm which submodules are consolidated under a parent** — especially in layered/stack codebases — so child layers get no `docs/` of their own.
3. Confirm module boundaries; offer to add/remove or re-parent modules.
4. Don't create files until scope is confirmed.
5. **Batch, don't interleave**: present the full proposed scaffold once and apply after a single approval — ideally batched into `ingest`'s Step 2.5 sign-off. For unattended runs, one sign-off for the whole batch before creating anything.

### Step 3: Gather Enough Context to Populate `docs/index.md`

> An empty or meaningless `index.md` is almost as bad as none. Populate each with real entries, even if sparse, so the `ingest` content stages can extend it.

1. Pick the docs **categories** that apply (list in `SKILL.md`). **Start small** — for a new or simple module a few pages may be enough; don't force every category.
2. Identify the concrete pages each category will hold; **trim the branch list to these placements** — never create a category branch just because it's listed.
3. If no content exists for a category, **don't create its directory** — defer to a later `ingest`.
4. Infer what you can from manifests, READMEs, and source layout.
5. **Ask the user when context is insufficient** (e.g., "What does `<module>` do and what are its boundaries?"). Collect high-level facts only.
6. Scaffold only `index.md` sections you can back with a nameable topic; no fabricated placeholder headings.

### Step 4: Create the Minimal Skeleton (Confirmation-Gated)

> See [readme-template.md](readme-template.md) and [index-templates.md](index-templates.md).

Create files only for branches holding pages — and **prefer one parent branch over many child-leaf branches**:

1. Create `README.md` entry points **only for affected modules**. For a parent absorbing child layers, write **one** parent `README.md` describing the whole component and linking to `docs/index.md`; leaf-layer submodules get none of their own.
2. Create a `docs/` + `docs/index.md` for each affected module and the root — where submodules are stack layers, use **one** parent `docs/index.md` to enumerate the sub-layers rather than nesting child `docs/` trees.
3. Create `docs/<category>/` subdirectories **only where pages will be placed** (Step 3).
4. Populate each `docs/index.md` with category headings and known page entries.
5. **Preserve existing content** and **avoid major generation**: don't overwrite a `README.md`, `docs/index.md`, or page without approval, and don't create full OKF pages or detailed summaries here — the `ingest` content stages (Steps 2-3 of `ingest-operation.md`) add those once the skeleton is confirmed.

---

## Verification Criteria

- [ ] Infers doc placement **read-only**: it created no files until scope was confirmed with the user.
- [ ] `docs/` roots were **consolidated high in the tree**: parent modules with layer submodules hold one `README.md` + `docs/` covering their children; leaf layers have no redundant `docs/`.
- [ ] Scaffold covered only **affected modules** and only `docs/<category>/` branches where pages will be placed.
- [ ] Only the minimal scaffold was created — no full OKF pages or architecture content.
- [ ] Every `docs/index.md` reflects real context (not fabrications) and lists only branches that hold pages.
- [ ] No existing documentation was overwritten without approval.
- [ ] Every module `README.md` has no OKF frontmatter and links to `docs/index.md`.
- [ ] Every created `docs/` directory contains an `index.md`.
