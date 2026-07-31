# Operation: `init` (Initialize Codebase-Embedded Wiki)

The `init` operation bootstraps a codebase-embedded wiki from scratch or fills missing module documentation gaps across an existing repository. It conducts codebase exploration, identifies logical modules, creates module entry `README.md` files (non-OKF) and `wiki/` subdirectories, generates high-level OKF wiki pages, constructs `wiki/index.md` Tables of Contents (up to 3 levels deep), and creates `.wiki-meta.json` with the current git commit hash.

---

## When to Run `init`

- Starting a new codebase-embedded wiki in a repository for the first time.
- Onboarding an un-documented or partially documented repository.
- Re-bootstrapping wiki structure and metadata after major repository restructuring.

---

## Detailed Step-by-Step Procedure

### Step 1: Discover Codebase Structure & Modules

1. Inspect the root directory structure and package manifests (`pom.xml`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, etc.).
2. Identify primary sub-packages, services, or logical modules (e.g., Maven modules, node projects, python packages, etc.).
3. For each module, locate key entry points, primary source files (`src/main/`), and test files (`src/test/`).
4. Check for pre-existing docs, architectural diagrams, or design notes provided by the user.

### Step 2: Extract High-Level Architecture & Domain Concepts

For each discovered module:

1. Analyze architectural decisions, domain responsibilities, trade-offs, and system boundaries.
2. Identify core concepts and design rationales (**why** decisions were made, **how** systems interact at a high level) that warrant dedicated wiki pages.
3. **DO NOT clone source code into Markdown**. AI agents can scan code directly for low-level logic and syntax. Focus purely on high-level mental models, data flows, and architectural context.
4. Formulate page titles, descriptions, and OKF tags (`type: concept`, `type: architecture`, `type: decision-record`, `type: guide`).

### Step 3: Create Module README Files (Non-OKF Entry Points)

> See [readme-template.md](readme-template.md) for the standard README template and decoupling rules.

For each module:

1. Ensure the module directory has a local `README.md` and a `wiki/` subdirectory.
2. Format `README.md` as a **standard Markdown file (NO OKF frontmatter)**.
3. Write concise sections:
   - **Scope & Purpose**: What the module does and its business boundaries.
   - **Usage & Integration**: How other modules interact with it and key configuration.
   - **Module Wiki Index Link**: Add a link pointing directly to the module wiki index.

### Step 4: Write Initial OKF Wiki Pages & `wiki/index.md` TOC

> See [okf-spec.md](okf-spec.md) for the OKF frontmatter schema and wiki page template, and [index-templates.md](index-templates.md) for the `wiki/index.md` TOC template.

For each identified topic/concept in a module:

1. Create OKF wiki pages within `<module>/wiki/` (e.g. `<module>/wiki/concepts/<topic-slug>.md`, `<module>/wiki/decisions/<adr-slug>.md`, etc.). Organize pages into subdirectories when beneficial, reflecting upper-level wiki categories/sections in the directory tree.
2. Include complete OKF frontmatter:
   - `type`: Enum (`architecture`, `decision-record`, `concept`, `guide`).
   - `title`: Human-readable title.
   - `description`: 1-2 sentence summary of high-level rationale.
   - `module`: Path to module.
   - `resource`: Path to main source file or directory.
   - `tags`: 2-5 descriptive taxonomy tags.
   - `timestamp`: Current ISO timestamp.
   - `related`: Markdown links to related OKF pages using relative paths.
3. Write structured Markdown content detailing context, design rationale, system mechanics (using text or Mermaid diagrams), and trade-offs. Avoid duplicating source code blocks.
4. Create `<module>/wiki/index.md` acting as a **Table of Contents up to 3 levels deep** (`# Title`, `## Section`, `### Sub-section`) indexing all wiki pages in the directory (including subdirectories) for rapid agent discovery.

### Step 5: Generate Minimal Tracking Manifest (`.wiki-meta.json`) & Global Wiki Index

> See [wiki-meta-schema.md](wiki-meta-schema.md) for the `.wiki-meta.json` schema and example.

1. Create or update `wiki/index.md` at the repository root detailing global architecture and linking to module `wiki/index.md` files (up to 3 levels deep).
2. Get the current git HEAD commit hash:

   ```bash
   git rev-parse HEAD
   ```

3. Create `.wiki-meta.json` at the root containing **only** essential tracking state:

   ```json
   {
     "version": "1.0.0",
     "last_sync_commit": "<CURRENT_GIT_COMMIT_SHA>",
     "last_sync_timestamp": "<CURRENT_ISO_8601_TIMESTAMP>"
   }
   ```

---

## Verification Criteria for `init`

- [ ] Every active module directory contains a `README.md` (without OKF frontmatter) detailing scope and usage, linking to `wiki/index.md`.
- [ ] Every `wiki/` directory contains an `index.md` Table of Contents indexed **up to 3 levels deep**.
- [ ] Wiki content focuses strictly on high-level rationale (**why/how**) and avoids code cloning.
- [ ] All OKF pages live strictly inside `wiki/` subdirectories (`wiki/**/*.md`) reflecting upper wiki levels in the file directory tree, and pass YAML frontmatter parsing.
- [ ] All file links and symbol references use relative Markdown paths.
- [ ] `.wiki-meta.json` exists at the root with minimal schema (`version`, `last_sync_commit`, `last_sync_timestamp`).
