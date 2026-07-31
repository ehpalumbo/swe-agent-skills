# Operation: `sync` (Synchronize Wiki with Repository Changes)

The `sync` operation ensures that the codebase-embedded wiki stays up to date with code changes applied to the repository since the last sync. It reads `last_sync_commit` from `.wiki-meta.json`, inspects `git diff <last_sync_commit> HEAD`, identifies changed modules, audits high-level OKF wiki pages and `wiki/index.md` TOCs, and updates `.wiki-meta.json` with the new commit hash and timestamp.

---

## When to Run `sync`

- Periodically during development or as part of CI/CD.
- Before embarking on a new feature or impact analysis to ensure wiki context is accurate.
- After pulling changes from remote or merging branches.

---

## Detailed Step-by-Step Procedure

### Step 1: Read Minimal Tracked Baseline

1. Read `.wiki-meta.json` at the root of the repository.
2. Extract `last_sync_commit`. If `.wiki-meta.json` does not exist or `last_sync_commit` is missing, inform the user and suggest running `init` first.

### Step 2: Determine Git Delta & Changed Modules

1. Execute git command to list modified, added, or deleted files since `last_sync_commit`:

   ```bash
   git diff --name-status <last_sync_commit> HEAD
   ```

2. Parse changed file paths and map them to their corresponding repository modules (e.g., changes to `auth-service/src/main/java/com/example/auth/JwtService.java` map to module `auth-service`).
3. If no relevant source files changed, output a message stating that the wiki is already up to date, and update `last_sync_timestamp` in `.wiki-meta.json`.

### Step 3: Audit High-Level Rationale & Conceptual Impact

For each affected module:

1. Identify OKF wiki pages in `<module>/wiki/**/*.md` that relate to the modified components.
2. Inspect if code changes alter high-level design decisions, system boundaries, security rules, or domain assumptions described in the wiki.
3. **DO NOT add code snippets or copy-paste code changes into the wiki**. Focus purely on updating high-level rationale (**why/how**).
4. Mark any OKF pages that require updates.

### Step 4: Apply Updates & Maintain `wiki/index.md` TOC

> See [okf-spec.md](okf-spec.md) for the OKF frontmatter schema and wiki page template, and [index-templates.md](index-templates.md) for the `wiki/index.md` TOC template.

For each affected OKF page:

1. Update content to reflect high-level architectural changes.
2. Ensure relative Markdown links point to valid files.
3. Update frontmatter `timestamp` to current ISO-8601 string.
4. **Update `wiki/index.md`**: If page titles or section headers changed, update the 3-level deep Table of Contents in `<module>/wiki/index.md`.
5. If a wiki page is no longer relevant due to major refactoring, archive or remove it from `<module>/wiki/`, and remove it from `wiki/index.md`.

### Step 5: Update Minimal Manifest Baseline

> See [wiki-meta-schema.md](wiki-meta-schema.md) for the `.wiki-meta.json` schema and example.

1. Get the current git HEAD commit hash:

   ```bash
   git rev-parse HEAD
   ```

2. Update `.wiki-meta.json` at root:

   ```json
   {
     "version": "1.0.0",
     "last_sync_commit": "<CURRENT_GIT_HEAD_SHA>",
     "last_sync_timestamp": "<CURRENT_ISO_8601_TIMESTAMP>"
   }
   ```

---

## Verification Criteria for `sync`

- [ ] `.wiki-meta.json` is updated with the current git HEAD SHA and timestamp.
- [ ] All OKF pages affected by structural or architectural code changes have been audited and updated.
- [ ] High-level focus (**why/how**) is maintained without cloning code.
- [ ] `wiki/index.md` Table of Contents (3 levels deep) is updated to reflect all current pages and headings.
- [ ] All wiki pages remain strictly inside `wiki/` subdirectories (`wiki/**/*.md`).
- [ ] All links use relative Markdown paths without absolute `file:///` URLs.
