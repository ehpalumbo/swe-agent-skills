# Implementation Plan: [Feature/Requirement Title or ID]

## Metadata

- **Author:** [Agent Name / ID]
- **Date:** [YYYY-MM-DD]
- **Structure:** [Single file / Phased (N phases)]

<!-- Omit inapplicable sections instead of filling them with placeholder text. -->

---

## References

Quick links to the documents behind this plan — the root artifact doubles as the entry point for later sessions:

- [Software Impact Analysis: Title](../reports/report_name.md)
- [Supporting Document / Analysis Report Title](path/to/document_name)

---

## Executive Summary

[Summary of the proposed changes and the confirmed approach.]

### Architecture

[Optional. Key structural decisions, components touched, and system constraints.]

### Assumptions

[Optional. Decisions adopted without user confirmation — e.g., proposed defaults taken during a non-interactive session.]

---

<!-- When Phased: keep the Phase Index and move all tasks into per-phase files (see phase-template.md); delete the Task Details section below. -->

## Phase Index

- **[Phase 1: Title]** - [one-line outcome] - [phase_1_plan.md](phases/phase_1_plan.md)
- **[Phase 2: Title]** - [one-line outcome] - [phase_2_plan.md](phases/phase_2_plan.md)

---

## Configuration & Environment Updates

- **Environment Variables:** [new/modified .env variables, or "None"]
- **Feature Flags:** [feature toggles gating the change, or "None"]
- **External Dependencies:** [new npm/pip packages or library upgrades, or "None"]

---

## Task Details

### [Component / Layer Name, e.g., Database, Backend, Frontend]

#### 1. [Imperative Task Title, e.g., Implement authentication middleware]

- **Prerequisites / Dependencies:** [e.g., None, or "Task 2 in this plan" — include any deferred-test dependency here]
- **Affected Files:**
  - [file_basename](relative/path/to/affected_file) - [NEW / MODIFY]
- **Affected Symbols:**
  - `ClassName` / `method_name()` / `database_table`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Observable outcome, e.g., Middleware returns 401 Unauthorized for invalid tokens]
  - [ ] [Observable outcome, e.g., Middleware attaches the user object to the request context]

#### 2. [Imperative Task Title]

- **Prerequisites / Dependencies:** [e.g., None]
- **Affected Files:**
  - [file_basename](relative/file/path)
- **Affected Symbols:**
  - `SymbolName`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Observable outcome]

---

## Verification Plan (Whole Feature)
<!-- This plan covers the whole feature. Every individual task is also verified against its own acceptance criteria. -->

### Automated Tests

[Unit, integration, or E2E tests to create/update, each mapped to the task it verifies]

### Manual Verification Steps

1. [Step 1, e.g., Start local environment and login]
2. [Step 2, e.g., Verify network payload contains new fields]

### Definition of Done (DoD)

- [ ] Code is formatted and linted (no warnings/errors)
- [ ] All automated unit & integration tests pass successfully
- [ ] No regression introduced to existing components
- [ ] Documentation is updated if applicable
