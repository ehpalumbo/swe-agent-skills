# Operation: `init` (Initialize Codebase-Embedded Wiki)

The `init` operation bootstraps the required codebase-embedded wiki structure by creating missing root and module `README.md` entry points, `wiki/` directories, `wiki/index.md` files, and minimal `.wiki-meta.json` tracking state. It is by default scoped to skeleton setup only; it does not perform a full repository scan or generate complete architecture content unless explicitly requested.

---

## When to Run `init`

- Starting a new codebase-embedded wiki in a repository for the first time.
- Onboarding an un-documented or partially documented repository.
- Re-bootstrapping wiki structure and metadata after major repository restructuring.

---

## Detailed Step-by-Step Procedure

### Step 1: Assess Existing Wiki Structure

1. Infer module structure from existing directory structure and project manifests (e.g., `package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, etc.).
2. Inspect the root directory for existing `README.md`, `.wiki-meta.json`, and `wiki/index.md`.
3. Check each module directory for a local `README.md`, `wiki/` subdirectory, and `wiki/index.md`.

### Step 2: Create Minimal Wiki Skeleton

For each missing or incomplete wiki location:

1. Create the required `wiki/` directory and `wiki/index.md` file at the root or module level.
2. Create missing module `README.md` entry points using the standard non-OKF README pattern.
3. Preserve any existing documentation and do not overwrite content unless the user approves an update.

### Step 3: Create Module README Files and Placeholder Indexes

> See [readme-template.md](readme-template.md) for the standard README template and decoupling rules.

For each affected module and the repository root:

1. Ensure the module directory has a local `README.md` and a `wiki/` subdirectory.
2. Format `README.md` as a **standard Markdown file (NO OKF frontmatter)**.
3. Write concise sections:
   - **Scope & Purpose**: What the module does and its business boundaries.
   - **Usage & Integration**: How other modules integrate or are consumed.
   - **Module Wiki Index Link**: Add a link pointing directly to the module wiki index.
4. Create `wiki/index.md` files at the root and module levels if missing.
5. Optionally include simple placeholder headings or a short table of contents in new `wiki/index.md` files, but do not generate complete architecture content automatically.

### Step 4: Preserve Existing Documentation and Avoid Major Generation

1. Preserve any existing `README.md`, wiki pages, or index files.
2. Do not overwrite existing content unless the user explicitly approves.
3. Do not create full OKF wiki pages or detailed architecture summaries as part of this skeleton bootstrap unless the user requests a deeper init operation.

---

## Verification Criteria for `init`

- [ ] Every module directory in scope contains a `README.md` (without OKF frontmatter) detailing scope and usage, linking to `wiki/index.md`.
- [ ] Every `wiki/` directory contains an `index.md` Table of Contents indexed **up to 3 levels deep**.
