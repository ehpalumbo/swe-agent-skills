# Operation: `query` (Navigate & Query Wiki Context)

The `query` operation allows AI agents or developers to navigate the codebase-embedded wiki to answer technical questions, build high-quality context for software tasks (such as impact analysis or implementation planning), and understand domain concepts without reading raw source code or uncurated RAG text.

---

## When to Run `query`

- Preparing context before running software engineering tasks or implementation planning.
- Answering architecture, design rationale, or domain logic questions about the codebase.
- Onboarding to a specific module or tracing system component boundaries.

---

## Detailed Step-by-Step Procedure

### Step 1: Parse Query Intent & Identify Target Scope

1. Analyze the user's question or dev task topic.
2. Extract key domain terms, module names, or architectural concepts.
3. Determine whether the query is:
   - **Module-Specific**: e.g., "Why was this design pattern chosen in `<module-name>`?"
   - **Cross-Cutting / Architectural**: e.g., "What is the event ordering strategy across services?"
   - **Design Record Inquiry**: e.g., "Why was `<technology-a>` chosen over `<technology-b>` for `<concern>`?"

### Step 2: Fast-Path Discovery via `wiki/index.md`

1. **Check Module Entry Point**: Read `<module>/README.md` for a high-level summary of module scope and usage value. Follow the link to `wiki/index.md`.
2. **Scan `wiki/index.md` Table of Contents**: Read the **3-level deep Table of Contents** (`# Title`, `## Section`, `### Sub-section`) in `<module>/wiki/index.md` (or root `wiki/index.md`).
3. **Target Specific OKF Pages**: Based on the 3-level TOC entries, jump directly to the target OKF page (`<module>/wiki/<path/to/page>.md`) that answers the query without parsing unrelated files.
4. **Follow OKF Metadata & Links**: Check OKF YAML frontmatter (`tags`, `type`, `description`, `related`) and follow relative links to gather complete context.

### Step 3: Verify Freshness & Staleness Status

1. Check `stale_after` fields in candidate OKF pages.
2. If `stale_after` has passed, inspect the underlying `resource` code file or run `git log` to confirm the wiki rationale remains accurate.

### Step 4: Synthesize Answer & Context Package

1. Formulate a direct, clear answer to the query highlighting **why** design decisions were made and **how** components operate at a high level.
2. Avoid dumping code blocks or verbatim source code into the response.
3. Provide relative Markdown links to:
   - Relevant OKF Wiki pages (e.g., `[<Page Title>](<module>/wiki/<section>/<page>.md)`).
   - Key source files (e.g., `[<filename>](<module>/src/<path/to/file>)`).

---

## Verification Criteria for `query`

- [ ] Leverages `wiki/index.md` (3-level deep TOC) for fast-path knowledge discovery.
- [ ] Synthesizes high-level architectural rationale (**why/how**) rather than dumping raw code snippets.
- [ ] Includes relative Markdown links to relevant OKF wiki pages and source code.
- [ ] Highlights any stale wiki pages encountered during traversal.
