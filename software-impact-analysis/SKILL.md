---
name: software-impact-analysis
description: Performs a comprehensive Software Impact Analysis for a feature request or bug fix on an existing codebase. Helps identify affected components, explore solution alternatives, evaluate risks, ask clarifying questions, and document the recommended way forward.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.1.0"
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
- Trace the execution path: entry point → modules/APIs/data flows → side effects. Record `file:line` evidence; do not declare scope isolated without caller/importer check.
- Produce the affected-components set directly (files, packages, tables, APIs + local vs. global scope + regression risks). Reuse existing patterns/helpers where found.
- Stop when the affected set stabilizes (no new callers/importers) or scope is clearly bounded; do not exhaustively read the repo for local changes.

### 3. Ask Clarifying Questions to the User

- Compile a list of specific, clear, and non-trivial questions to resolve open questions, ambiguity, or design trade-offs.
- Avoid asking questions that can be answered by studying the codebase; focus on product behavior, design choices, or business logic.

### 4. Evaluate Solution Approaches

- Define at least two implementation strategies (e.g., a direct/minimal change vs. a more robust/refactored design).
- For each approach, document:
  - High-level design and how it works.
  - Pros (simplicity, execution speed, performance, etc.).
  - Cons (technical debt, complexity, maintenance effort, etc.).
  - Risk Level (Low/Medium/High) and potential regression points.

### 5. Recommend Way Forward

- Select the best solution approach based on the trade-offs evaluated.
- Provide a clear, technical rationale for why this approach was chosen.
- Outline the high-level sequence of steps to execute the plan.

---

## Output Format

The final deliverable of this skill must be a Software Impact Analysis Report.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) **only when you are ready to write the final report**, then:

1. Fill out all sections of the template based on your findings and analysis.
2. Save or present the report to the user as requested.

### Self-Check (before delivering)

Before presenting the report:

- Verify that every file, symbol, and table listed in the "Affected Components" table actually exists in the codebase.
- Verify that every "Remaining Open Question" is genuinely unanswerable from code — if it can be resolved by studying the codebase, resolve it instead of deferring it.
- Re-walk the codebase for any component that was not directly verified (e.g., relying on a symbol name without confirming its callers or importers).

Fix any discrepancies and repeat until the report passes all checks before finalizing.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Trusting symbol names is not enough.** A function may be called from many places — always hunt down its callers before declaring a change isolated.
- **A shared/module-level util can ripple across multiple features.** Check all importers, not just the one named in the request.
- **Do not skip the clarifying-questions step for ambiguous requirements just because the request is short.** Product behavior and trade-offs are rarely inferable from code alone.
