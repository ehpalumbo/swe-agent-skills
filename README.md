# Software Engineering Skills for Agentic Development

This repository is a collection of Agent Skills that have been designed and refined to support software engineering with AI agents (agentic development). They support a pragmatic approach to leveraging AI agents for software development, with a specific focus on **brownfield development** (working with existing codebases).

These skills have been iteratively improved with learnings from daily use in a professional software engineering environment. They are opinionated and reflect the best practices of the author, but may serve as inspiration for building your own agentic development workflow or as a starting point for contributing your own skills to this repository.

## What are Agent Skills?

[Agent Skills](https://agentskills.io/) are structured, reusable instruction templates designed to equip AI agents with specific capabilities.

* **Why use them:** AI agents are highly capable but can exhibit variable behavior or skip critical analysis steps. Standardizing skills ensures agents execute tasks systematically, consistently, and with proper checks.
* **How these skills adhere:** Each skill in this repository is packaged as a markdown file with standardized frontmatter metadata, a defined operational procedure, and structured output templates. This makes them easy for AI agents to parse, load, and execute reliably.

## Included Skills

### 1. [Software Impact Analysis](./software-impact-analysis/SKILL.md)

* **Purpose:** Pre-code analysis to evaluate requirements, map affected files/symbols, assess side effects, and propose technical design options.
* **When to use:** Use before implementing any feature request, bug fix, or refactoring.
* **Output:** A structured impact analysis report detailing pros/cons of proposed approaches, regression risks, and open questions.

### 2. [Implementation Planning](./implementation-planning/SKILL.md)

* **Purpose:** Translates requirements or impact analyses into concrete, step-by-step development tasks.
* **When to use:** Use after design/architecture is approved to plan coding execution in manageable, reviewable slices.
* **Output:** An actionable implementation plan listing tasks with affected files, clear acceptance criteria, and verification steps.

## Usage & Agent Integration

These skills can be imported or referenced by AI agents supporting software development (such as GitHub Copilot, Claude Code, Antigravity, or custom agent systems). They provide checklists and templates that ensure consistency, prevent regressions, and align implementation choices with human expectations before any code is written.

## Contributing

We welcome contributions of new software engineering skills or improvements to existing ones.

### Structure Guidelines

* **Directory Structure:** Each skill must live in its own directory with a lowercase, hyphen-separated name, containing a single `SKILL.md` (e.g., `code-refactoring/SKILL.md`).

* **Standard YAML Frontmatter:** Every `SKILL.md` must start with a YAML frontmatter block detailing metadata:

  ```yaml
  ---
  name: unique-skill-name
  description: A short description of the skill and when it is useful.
  license: Apache-2.0
  metadata:
    author: your-username
    version: "1.0.0"
  ---
  ```

* **Consistent Layout:** The body of the skill should follow a standard layout:
  1. `# Skill Title`
  2. `## When to Use`
  3. `## Procedure` (numbered sequential steps)
  4. `## Output Format` (including a Markdown template code block)
