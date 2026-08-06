# Operation: `init` (Initialize Codebase-Embedded Documentation)

> [!IMPORTANT]
> `init` is deliberately **minimalistic**. On large multi-module repositories a full scan and wholesale documentation generation is expensive and rarely desired. `init` scaffolds only the lightweight structure needed to begin ingesting knowledge: module `README.md` entry points, `docs/` directories, and `docs/index.md` indexes. It does **not** generate full OKF docs pages or deep architecture content. If you later need content, add it incrementally via the [`ingest`](ingest-operation.md) operation.

---

## When to Run `init`

- Starting codebase-embedded docs in a repository for the first time.
- Onboarding an un-documented or partially documented repository.
- Re-bootstrapping docs after major restructuring.

---

## Detailed Step-by-Step Procedure

### Step 1: Infer the Affected Module Branches (Read-Only)

1. Inspect the root directory for an existing `README.md` and `docs/` directory.
2. Infer the module structure from directory layout and project manifests (e.g., `package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, `pyproject.toml`). Do **not** attempt an exhaustive file-by-file scan of large trees.
3. Produce a proposed list of **affected branches**: the repository root plus each inferred module target that has a meaningful reason to hold docs. Each entry records its detected `README.md` / `docs/` presence.

> **Scope to where docs will actually live.** Never scaffold structure for empty modules or for modules that are not affected by the requested skill operation. Only create a branch when the requested operation actually needs to place documentation content there.
>
> Keep discovery **read-only**. Do not create, move, or modify any files during this step.

### Step 2: Confirm the Inferred Module Structure with the User

1. Present the affected branches (root + modules) and the planned scaffold for each (e.g., `README.md`, `docs/`, `docs/index.md`).
2. **Ask the user to confirm the module boundaries are correct** before any file is created, especially for large multi-module repositories. Offer to add/remove modules based on their feedback.
3. Do not proceed to file creation until the scope is confirmed.

### Step 3: Gather Enough Context to Populate `docs/index.md`

> An empty or meaningless `index.md` is almost as bad as no index at all. Populate each index with real section/page entries, even if sparse, so later `ingest` runs can extend it.

1. For each affected module, determine the docs **categories** that apply (see the category list in `SKILL.md`, e.g., `specs/`, `architecture/`, `decisions/`, `domain/`, `workflows/`, `guides/`).
2. For each category, identify the concrete pages the requested operation will place there. Keep the branch list **trimmed to these actual placements** — do not create a category branch just because it is a listed category.
3. Gather sufficient context to seed each `index.md`: infer what you can from manifests, existing READMEs, and obvious source layout.
4. **Ask the user for details when context is insufficient** — e.g., "What does `<module-name>` do and what are its boundaries?" or "Which of these sections should I scaffold for `<module>`?" Collect concise, high-level facts; do not interrogate at implementation depth.
5. Only scaffold `index.md` sections you can actually back with a nameable topic; leave-out categories that do not apply rather than fabricating placeholder headings.

### Step 4: Create the Minimal Skeleton (Confirmation-Gated)

> See [readme-template.md](readme-template.md) for the standard README template and decoupling rules, and [index-templates.md](index-templates.md) for index templates.

Now that structure is confirmed and context gathered, create files only for the branches where docs pages will be placed:

1. Create missing module `README.md` entry points **only for affected modules** using the standard **non-OKF** README pattern (scope & purpose, usage & integration, link to `docs/index.md`).
2. Create a `docs/` directory and a `docs/index.md` for each affected module and the repository root.
3. Create `docs/<category>/` subdirectories **only for the branches where docs pages will actually be placed**, as identified in Step 3. Never create a category subdirectory for empty or unrequested content.
4. Populate each `docs/index.md` with the category headings and known page entries gathered in Step 3 — listing only branches that will hold pages.
5. **Preserve existing content**: do not overwrite an existing `README.md`, `docs/index.md`, or docs page unless the user explicitly approves an update.

### Step 5: Preserve Existing Documentation & Avoid Major Generation

1. Preserve any existing `README.md`, docs pages, or index files.
2. Do not create full OKF docs pages or detailed architecture summaries during this skeleton bootstrap.
3. As the skeleton is only the entry point for knowledge, defer all content distillation to the [`ingest`](ingest-operation.md) operation.

---

## Verification Criteria

- [ ] The inferred module structure was **confirmed with the user** before any files were created.
- [ ] Scaffolding covered only **affected modules** and only the **`docs/<category>/` branches where docs pages will be placed** — nothing was created for empty or unrequested modules/branches.
- [ ] Only the minimal scaffold was created (missing `README.md`, `docs/`, `docs/index.md`) — no full OKF pages or architecture content.
- [ ] Every created `docs/index.md` reflects the module's confirmed/supplied context, not empty fabrications, and lists only branches that hold pages.
- [ ] No existing documentation was overwritten without explicit user approval.
- [ ] Every module `README.md` uses no OKF frontmatter and links to `docs/index.md`.
- [ ] Every created `docs/` directory contains an `index.md`.
