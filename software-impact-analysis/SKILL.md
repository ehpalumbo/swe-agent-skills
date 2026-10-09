---
name: software-impact-analysis
description: Performs a comprehensive Software Impact Analysis for a feature request or bug fix on an existing codebase. Helps identify affected components, explore solution alternatives, evaluate risks, ask clarifying questions, and document the recommended way forward.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.2.0"
---

# Software Impact Analysis

Use this skill to conduct a thorough impact analysis when presented with a feature request, bug report, or refactoring proposal. This process ensures all modifications are well-planned, risks are managed, and dependencies or regressions are identified before writing code.

---

## When to Use

- **Feature Requests:** Before implementing new features or extending existing functionality to understand where the changes fit.
- **Bug Fixes:** Before making code modifications to resolve a bug, to verify we fix the root cause and avoid introducing regressions.
- **Refactoring & Architectural Changes:** When modifying internal structures, APIs, or database schemas.
- **Ambiguous Requirements:** When a task is high-level or underspecified, prompting the need to explore the codebase and ask clarifying questions first.

---

## Procedure

Follow these steps during the analysis — iterate and parallelize exploration where possible:

### 1. Understand the Requirement

- Analyze the user request, bug report, or issue description.
- Extract the core objective, success criteria, and any explicit constraints (e.g., performance, technology stack, security).
- Identify any assumptions or ambiguities in the requirement that need validation.

### 2. Evidence-Grounded Exploration (iterate, parallelize where possible)

- Run parallel searches for entry points, related logic, callers/importers, configs, docs/schemas, and covering tests. Use subagents for independent areas in large codebases.
- Trace the execution path: entry point → modules/APIs/data flows → side effects. Record `path:line` evidence per affected component (Evidence column of the report's Affected Components table); do not declare scope isolated without caller/importer check.
- Produce the affected-components set directly (files, packages, tables, APIs + local vs. global scope + regression risks). Reuse existing patterns/helpers where found.
- Check git history (`log`, `blame`, recent PRs) for churn and hotspots in affected areas to calibrate risk.
- Ground bug fixes in execution: reproduce with a script/logs before proposing a fix. If the bug cannot be reproduced in the current environment, record the attempted repro steps, the blocker, and proceed with the analysis at reduced confidence rather than skipping verification. Note existing tests covering affected areas and coverage gaps.
- Stop when the affected set stabilizes (no new callers/importers) or scope is clearly bounded; do not exhaustively read the repo for local changes.

### 3. Ask Clarifying Questions

- Ask whatever is needed to resolve ambiguity in scope, behavior, constraints, or design trade-offs that cannot be answered from code or search. There is no fixed cap — ask as you go rather than saving questions for the end, grouping related ones.
- Give each question a proposed default. A default lets the user confirm quickly and keeps the session moving instead of stalling on every open decision.
- Record ambiguities that survive the conversation as Assumptions (report §4).
- Skip questioning only when the requirement is unambiguous with no open decisions.

### 4. Evaluate Solution Approaches (risk-proportional)

- **Tier 1 — concise:** trivial or local, with no security, auth/crypto/access-control, data-migration, or public-API-contract impact. One approach is enough; note rejected alternatives in one line.
- **Tier 2 — full:** everything else — non-trivial, cross-cutting, API, DB, or complex bug, plus any security-sensitive, auth/crypto/access-control, or migration change regardless of size. Define at least two strategies (e.g., direct/minimal vs. robust/refactored).
- For each approach, document:
  - High-level design and how it works.
  - Pros (simplicity, execution speed, performance, etc.).
  - Cons (technical debt, complexity, maintenance effort, etc.).
  - Risk Level (Low/Medium/High) and potential regression points.

### 5. Recommend Way Forward

- Select the best approach with technical rationale (trade-offs, scalability, maintenance).
- Outline the high-level execution sequence only — hand off to the `implementation-planning` skill for phased tasks; do not write detailed tasks here.
- State effort estimate as t-shirt size based on complexity, not time (S = low complexity, 1–2 files/single component, covered by existing tests; M = medium complexity, multiple components, new tests + regression coverage needed; L = high complexity, cross-cutting/architectural, migration or contract changes, extensive testing/rollout care) and rollback/migration needs if applicable; record in report §4 Recommended Way Forward.

---

## Output Format

The final deliverable of this skill must be a Software Impact Analysis Report.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) **only when you are ready to write the final report**, then:

1. Fill out the applicable sections of the template. Depth follows the tiers defined in §4:
   - **Tier 1 (concise):** Executive Summary, Affected Components, and Verification & Testing Plan are sufficient.
   - **Tier 2 (full):** the full template, including Approaches, Security/Performance, and Rollback.
   Regardless of tier, the report must carry an Effort Estimate and Assumptions — never drop them for brevity.
2. Save or present the report to the user as requested. Omit inapplicable subsections instead of filling with placeholders.

### Self-Check (before delivering)

Before presenting the report:

- Every affected file cites a verified `path:line` on disk in the Affected Components table's Evidence column; every symbol was confirmed via caller/importer search, not inferred from its name.
- Every "Remaining Open Question" is genuinely unanswerable from code/search — if resolvable, resolve it instead of deferring it.
- No unrefilled template placeholders (e.g. `[file/path]`, `TBD`) remain; omitted subsections are intentional per the §4 tiers, not incomplete work.

Run this checklist once before presenting the report.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Trusting symbol names is not enough.** A function may be called from many places — always hunt down its callers before declaring a change isolated.
- **A shared/module-level util can ripple across multiple features.** Check all importers, not just the one named in the request.
