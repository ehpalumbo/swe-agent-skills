---
name: implementation-planning
description: Translates approved requirements, specifications, or prior analysis into an implementation plan — tasks with affected files and symbols, tests paired with the code they verify, and testable acceptance criteria. Produces a single report for one increment, or an index linking one document per increment otherwise. Use after the solution design/approach is defined; not for impact analysis or for writing the code itself.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "2.0.0"
---

# Implementation Planning

Turn approved requirements, specifications, or prior analysis into a concrete implementation plan. Every task names the files and symbols it touches, carries acceptance criteria that can be checked objectively, and pairs with the tests that verify it, so the work can be reviewed and steered one increment at a time.

---

## When to Use

- **Prior analysis or design:** after an approach is approved, to translate it into tasks.
- **Explicit requirements:** when well-defined requirements are provided and a plan is requested directly.
- **Multi-module changes:** before coding work that spans modules or layers, or needs a staged rollout.

Not for scoping, risk assessment, or design alternatives, and not for writing the code itself.

---

## Procedure

### 1. Lock the inputs

- Adopt the approved approach in the input materials as-is; do not re-derive or renegotiate it here.
- If the inputs contain no concrete design, propose one — rationale, architectural trade-offs, and security/performance/integration impact — and get confirmation before planning. In a non-interactive session, adopt your proposed default and record it as an assumption in the plan.
- Extract scope, success criteria, and constraints. Ask the user only about ambiguities that block planning, and give each question a proposed default.

### 2. Verify the current codebase state

- Re-verify on disk every path and symbol carried over from the requirements, specification, or prior analysis — that material is a snapshot and goes stale (branch switches, concurrent edits).
- Explore directly whatever the inputs did not cover; cite `path:line` evidence for files to be modified.

### 3. Size the plan

Default to a **single report** covering all tasks when the work fits in one reviewable commit — one review at the end is enough. Split into multiple increments (an index file linking one file per increment) when intermediate user review could help prevent rework: the work has multiple independently verifiable milestones, or a wrong direction taken early would be expensive to undo. Each increment is one user checkpoint — implement it, review it, steer before the next begins — and each increment file must stand alone enough to hand to an agent with only its Overview for context. When unsure, stay single-report.

### 4. Slice into increments

- Prefer **vertical slices** (an end-to-end flow for a subset of the requirements) when applicable; then split by deliverable component.
- One increment = one reviewable commit = one user checkpoint. Group the tasks of each commit under one increment; split a task that is too large to review on its own, and never group two separately reviewable commits into one increment.
- Give each increment work the user can evaluate on its own — working behavior or an inspectable foundation later increments build on.

### 5. Write the plan

- Give every task an imperative title, affected modules and files by exact path, and affected symbols by name.
- Pair each implementation task with the task that tests it — same increment, same task when possible. Defer a test only when it cannot run until later changes land (e.g., E2E needing the full flow wired up), and state that dependency in **Prerequisites / Dependencies**.
- Write acceptance criteria as checkbox items naming observable outcomes (`returns 401 for invalid tokens`), never "works correctly" or "handles errors".

---

## Output Format

The deliverable is an implementation plan: a single report file for single-increment work, or an index file linking one file per increment for multi-increment work.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) for the single report (or index), and [`assets/increment-template.md`](assets/increment-template.md) for each increment file in multi-increment plans — **only when you are ready to write the deliverable**. Fill them in as follows:

1. Single-increment plans: fill the report template as one self-contained report — keep its **Task Details** with all tasks (tests included) and its **Verification Plan**; delete the **Increment Index**.
2. Multi-increment plans: use the report template as the index — keep the **Increment Index** and move all tasks into per-increment files; delete the report's **Task Details** section. Increment files each open with an **Overview** that places the increment in the big picture and states what the user should check before approving the next increment, followed by the increment's tasks (tests included) and an increment-level **Verification Plan** that is the checkpoint review.
3. Omit inapplicable sections instead of filling them with placeholder text.
4. Save the plan where the user asks; otherwise alongside the source requirements, specification, or prior analysis, or under `docs/implementation-plans/<feature>/` when there is no adjacent convention.

### Self-Check (before delivering)

Run this checklist once, fix what fails, and repeat until it passes:

- [ ] Every affected path resolves on disk; every symbol was confirmed by reading its definition or callers — none inferred from its name.
- [ ] Every acceptance criterion names a specific, observable outcome.
- [ ] Every increment is a single reviewable commit and a single user checkpoint.
- [ ] Every feature task has its test task in the same or an earlier increment, or an explicitly justified deferral.
- [ ] Single-increment plans: the single report carries **Task Details** and its own **Verification Plan**; no **Increment Index** remains.
- [ ] Multi-increment plans: the index links resolve to real relative paths, and each increment file opens with an `Overview` section and carries its own **Verification Plan** runnable without later increments' changes.
- [ ] No unfilled template placeholders remain; inapplicable sections were removed, not stubbed.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Input materials are snapshots, not ground truth.** Paths and symbols verified during prior analysis can be stale by planning time — reconfirm them on disk before they become tasks.
- **Layer-by-layer slicing is a smell.** "Backend, then frontend, then tests" produces increments nobody can verify alone; slice vertically and split by component only when vertical is not feasible.
- **Tests travel with their code.** An increment whose diff does not include the tests that verify it is the wrong increment; the only exception is a test that needs a later increment, and that dependency belongs in the task's prerequisites.
