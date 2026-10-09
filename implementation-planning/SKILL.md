---
name: implementation-planning
description: Translates approved requirements or a completed impact analysis into an implementation plan — tasks with affected files and symbols, tests paired with the code they verify, and testable acceptance criteria. Produces a single plan file by default and phase files only when the work exceeds one reviewable commit. Use after the design/approach is approved; not for impact analysis (see `software-impact-analysis`) or for writing the code itself.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "2.0.0"
---

# Implementation Planning

Turn approved requirements — or a completed Software Impact Analysis — into a concrete implementation plan. Every task names the files and symbols it touches, carries acceptance criteria that can be checked objectively, and pairs with the tests that verify it, so implementation proceeds in increments that can each be reviewed and shipped.

---

## When to Use

- **Post-impact analysis:** after a Software Impact Analysis is approved, to translate its recommended approach into tasks.
- **Explicit requirements:** when well-defined requirements are provided and a plan is requested directly.
- **Multi-module changes:** before coding work that spans modules or layers, or needs a staged rollout.

Not for scoping, risk assessment, or design alternatives (use `software-impact-analysis`), and not for writing the code itself.

---

## Procedure

### 1. Lock the inputs

- Adopt the approach approved in the impact analysis as-is; do not re-derive or renegotiate it here.
- If the inputs contain no concrete design, propose one — rationale, trade-offs, and security/performance/integration impact — and get confirmation before planning. In a non-interactive session, adopt your proposed default and record it as an assumption in the plan.
- Extract scope, success criteria, and constraints. Ask the user only about ambiguities that block planning, and give each question a proposed default.

### 2. Verify the current codebase state

- Re-verify on disk every path and symbol carried over from the analysis — that output is a snapshot and goes stale (branch switches, concurrent edits).
- Explore directly whatever the analysis did not cover; cite `path:line` evidence for files to be modified.

### 3. Size the plan

Default to a **single plan file** covering all tasks. Phase (an index file linking one file per phase) only when the work exceeds one reviewable commit *and* cannot be understood in one read — e.g., a staged rollout across components. When unsure, stay single-file.

### 4. Slice into increments

- Prefer **vertical slices** (an end-to-end flow for a subset of the requirements); split by deliverable component only when vertical slicing is not feasible.
- One increment = one reviewable commit. Split a task that is too large to review on its own instead of merging it into a giant phase.

### 5. Write the plan

- Give every task an imperative title, affected files by exact path, and affected symbols by name.
- Pair each implementation task with the task that tests it — same increment, same task when possible. Defer a test only when it cannot run until later changes land (e.g., E2E needing the full flow wired up), and state that dependency in **Prerequisites / Dependencies**.
- Write acceptance criteria as checkbox items naming observable outcomes (`returns 401 for invalid tokens`), never "works correctly" or "handles errors".

---

## Output Format

The deliverable is an implementation plan: a single markdown file, or an index file linking per-phase files when phased.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) for the main file (or index), and [`assets/phase-template.md`](assets/phase-template.md) for each phase file when phasing — **only when you are ready to write the deliverable**. Fill them in as follows:

1. Phase files each open with an **Overview** that places the phase in the big picture, followed by the phase's tasks (same task format as the main template, tests included) and a phase-level **Verification Plan**.
2. Omit inapplicable sections instead of filling them with placeholder text.
3. Save the plan where the user asks; otherwise beside the impact analysis report, or under `docs/implementation-plans/<feature>/` when neither convention applies.

### Self-Check (before delivering)

Run this checklist once, fix what fails, and repeat until it passes:

- [ ] Every affected path resolves on disk; every symbol was confirmed by reading its definition or callers — none inferred from its name.
- [ ] Every acceptance criterion names a specific, observable outcome.
- [ ] Every increment (phase, when phased) is a single reviewable commit.
- [ ] Every feature task has its test task in the same or an earlier increment, or an explicitly justified deferral.
- [ ] When phased: the index links resolve to real relative paths, and each phase file opens with an `Overview` section.
- [ ] No unfilled template placeholders remain; inapplicable sections were removed, not stubbed.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Analysis output is a snapshot, not ground truth.** Paths and symbols verified during impact analysis can be stale by planning time — reconfirm them on disk before they become tasks.
- **Layer-by-layer phasing is a smell.** "Backend, then frontend, then tests" produces increments nobody can ship or verify alone; slice vertically and split by component only when vertical is not feasible.
- **Tests travel with their code.** A phase whose diff does not include the tests that verify it is the wrong phase; the only exception is a test that needs a later increment, and that dependency belongs in the task's prerequisites.
