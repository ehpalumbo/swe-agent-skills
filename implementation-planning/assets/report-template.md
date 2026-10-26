# Implementation Plan: [Feature/Requirement Title or ID]

## Metadata

- **Author:** [Agent Name / ID]
- **Date:** [YYYY-MM-DD]
- **Structure:** [Single increment / N increments]

<!-- Omit inapplicable sections instead of filling them with placeholder text. -->

---

## References

Quick links to the inputs behind this plan — the root artifact doubles as the entry point for later sessions:

- [Requirements / specification / prior analysis: Title](path/to/source_document)
- [Supporting document: Title](path/to/supporting_documentation)

---

## Executive Summary

[Summary of the proposed changes and the confirmed approach.]

### Architecture

[Optional. Key structural decisions, components touched, and system constraints.]

### Assumptions

[Optional. Decisions adopted without user confirmation — e.g., proposed defaults taken during a non-interactive session.]

---

<!-- When single increment: delete the Increment Index below and keep Task Details. When multi-increment: keep the Increment Index and move all tasks into per-increment files (see increment-template.md); delete the Task Details section below. -->

## Increment Index

- **[Increment 1: Title]** - [one-line outcome] - [increment_1_plan.md](increments/increment_1_plan.md)
- **[Increment 2: Title]** - [one-line outcome] - [increment_2_plan.md](increments/increment_2_plan.md)

---

## Configuration & Environment Updates

<!-- Omit this section or any item with nothing to report; never write "None". -->

- **Environment Variables:** [new/modified .env variables]
- **Feature Flags:** [feature toggles gating the change]
- **External Dependencies:** [new dependencies, packages or library upgrades]

---

## Task Details

<!-- Single-increment plans: keep this section with all tasks below. Multi-increment plans: delete this section and move all tasks into per-increment files (see increment-template.md). -->

### 1. [Imperative Task Title, e.g., Implement authentication middleware]

- **Prerequisites / Dependencies:** [e.g., None, or "Task 2 in this plan" — include any deferred-test dependency here]
- **Affected Files:**
  - [file_basename](relative/path/to/affected_file) - [NEW / MODIFY]
- **Affected Symbols:**
  - `ClassName` / `method_name()` / `database_table`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Observable outcome, e.g., Middleware returns 401 Unauthorized for invalid tokens]
  - [ ] [Observable outcome, e.g., Middleware attaches the user object to the request context]

<!-- Repeat the `###` task block for each further task. -->

---

## Verification Plan (Whole Feature)
<!-- This plan covers the whole feature. Every individual task is also verified against its own acceptance criteria. -->

### Automated Tests

[Unit, integration, or E2E tests to create/update, each mapped to the task it verifies — reference tasks as "Increment N, Task K"]

### Manual Verification Steps

1. [Step 1, e.g., Start local environment and login]
2. [Step 2, e.g., Verify network payload contains new fields]

### Definition of Done (DoD)

- [ ] Code is formatted and linted (no warnings/errors)
- [ ] All automated unit & integration tests pass successfully
- [ ] No regression introduced to existing components
- [ ] Documentation is updated if applicable
