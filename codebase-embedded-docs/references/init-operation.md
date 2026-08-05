# Operation: `init` (Initialize Codebase-Embedded Documentation)

The `init` operation bootstraps the required codebase-embedded documentation structure by creating missing root and module `README.md` entry points, `docs/` directories and `docs/index.md` files. It is by default scoped to skeleton setup only; it does not perform a full repository scan or generate complete architecture content unless explicitly requested.

---

## When to Run `init`

- Starting a new codebase-embedded docs structure in a repository for the first time.
- Onboarding an un-documented or partially documented repository.
- Re-bootstrapping docs structure and metadata after major repository restructuring.

---

## Detailed Step-by-Step Procedure

### Step 1: Assess Existing Docs Structure

1. Infer module structure from existing directory structure and project manifests (e.g., `package.json`, `pom.xml`, `Cargo.toml`, `go.mod`, etc.).
2. Inspect the root directory for existing `README.md` and `docs/` directory.
3. Check each module directory for a local `README.md`, `docs/` subdirectory, and `docs/index.md`.

### Step 2: Create Minimal Docs Skeleton

For each missing or incomplete docs location:

1. Create the required `docs/` directory and `docs/index.md` file at the root or module level.
2. Create missing module `README.md` entry points using the standard non-OKF README pattern.
3. Preserve any existing documentation and do not overwrite content unless the user approves an update.

### Step 3: Create Module README Files and Placeholder Indexes

> See [readme-template.md](readme-template.md) for the standard README template and decoupling rules.

For each affected module and the repository root:

1. Ensure the module directory has a local `README.md` and a `docs/` subdirectory.
2. Format `README.md` as a **standard Markdown file (NO OKF frontmatter)**.
3. Write concise sections:
   - **Scope & Purpose**: What the module does and its business boundaries.
   - **Usage & Integration**: How other modules integrate or are consumed.
   - **Module Docs Index Link**: Add a link pointing directly to the module docs index.
4. Create `docs/index.md` files at the root and module levels if missing.
5. Optionally include simple placeholder headings or a short table of contents in new `docs/index.md` files, but do not generate complete architecture content automatically.

### Step 4: Preserve Existing Documentation and Avoid Major Generation

1. Preserve any existing `README.md`, docs pages, or index files.
2. Do not overwrite existing content unless the user explicitly approves.
3. Do not create full OKF docs pages or detailed architecture summaries as part of this skeleton bootstrap unless the user requests a deeper init operation.

---

## Verification Criteria for `init`

- [ ] Every module directory in scope contains a `README.md` (without OKF frontmatter) detailing scope and usage, linking to `docs/index.md`.
- [ ] Every `docs/` directory contains an `index.md` Table of Contents indexed **up to 3 levels deep**.
