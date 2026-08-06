# Operation: `init` (Initialize Codebase-Embedded Documentation)

> [!IMPORTANT]
> `init` is deliberately **minimalistic** — full scans and wholesale generation are expensive and rarely wanted. It scaffolds only what's needed to begin ingesting: module `README.md` entry points, `docs/` directories, and `docs/index.md` indexes. **Start small and grow on demand**: create `docs/<category>/` branches only when the content being placed justifies them; add more later via [`ingest`](ingest-operation.md). No full OKF pages or deep architecture content here.

---

## When to Run `init`

- First-time docs on a repository.
- Onboarding an undocumented or partially documented repository.
- Re-bootstrapping after major restructuring.

---

## Step-by-Step Procedure

### Step 1: Infer Affected Module Branches (Read-Only)

1. Check the root for an existing `README.md` and `docs/`.
2. Infer modules from the directory layout and manifests (`package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, `pyproject.toml`). Don't exhaustively scan large trees.
3. Produce a proposed list of **affected branches** (root + each module with a reason to hold docs), noting each entry's `README.md` / `docs/` presence.

> **Scope to where docs will actually live** — never scaffold for empty or unaffected modules. Keep this step **read-only**.

### Step 2: Confirm the Inferred Module Structure with the User

1. Present the affected branches and the planned scaffold for each (`README.md`, `docs/`, `docs/index.md`).
2. **Confirm module boundaries** before creating anything, especially for large repos. Offer to add/remove modules.
3. Do not create files until scope is confirmed.

### Step 3: Gather Enough Context to Populate `docs/index.md`

> An empty or meaningless `index.md` is almost as bad as none. Populate each with real entries, even if sparse, so later `ingest` runs can extend it.

1. Pick the docs **categories** that apply (list in `SKILL.md`). **Start small** — for a new or simple module a few pages may be enough; don't force every category.
2. Identify the concrete pages each category will hold; **trim the branch list to these placements** — never create a category branch just because it's listed.
3. If no content exists for a category, **don't create its directory** — defer to a later `ingest`.
4. Infer what you can from manifests, READMEs, and source layout.
5. **Ask the user when context is insufficient** (e.g., "What does `<module>` do and what are its boundaries?"). Collect high-level facts only.
6. Scaffold only `index.md` sections you can back with a nameable topic; no fabricated placeholder headings.

### Step 4: Create the Minimal Skeleton (Confirmation-Gated)

> See [readme-template.md](readme-template.md) and [index-templates.md](index-templates.md).

Create files only for branches where pages will be placed:

1. Create missing module `README.md` entry points **only for affected modules** (scope & purpose, usage & integration, link to `docs/index.md`).
2. Create a `docs/` directory and `docs/index.md` for each affected module and the root.
3. Create `docs/<category>/` subdirectories **only where pages will actually be placed** (Step 3).
4. Populate each `docs/index.md` with category headings and known page entries — only branches holding pages.
5. **Preserve existing content**: don't overwrite a `README.md`, `docs/index.md`, or page unless the user approves.

### Step 5: Preserve Existing Content & Avoid Major Generation

1. Preserve existing READMEs, pages, and indexes.
2. Don't create full OKF pages or detailed summaries here; defer to [`ingest`](ingest-operation.md).

---

## Verification Criteria

- [ ] Inferred module structure was **confirmed with the user** before any files were created.
- [ ] Scaffold covered only **affected modules** and only **`docs/<category>/` branches where pages will be placed**.
- [ ] Only the minimal scaffold was created — no full OKF pages or architecture content.
- [ ] Every `docs/index.md` reflects real context (not fabrications) and lists only branches that hold pages.
- [ ] No existing documentation was overwritten without approval.
- [ ] Every module `README.md` has no OKF frontmatter and links to `docs/index.md`.
- [ ] Every created `docs/` directory contains an `index.md`.