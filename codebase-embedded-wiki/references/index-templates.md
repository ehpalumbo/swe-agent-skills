# Wiki Index Templates (`wiki/index.md`)

Every `wiki/` directory MUST contain an `index.md` Table of Contents indexed **up to 3 levels deep** (`# Title`, `## Section`, `### Sub-section`). This allows AI agents to rapidly discover wiki structure without reading individual pages.

> [!IMPORTANT]
> Subdirectories inside `wiki/` reflect top-level TOC sections. Whenever new wiki pages or major section headers are added or modified, the corresponding `wiki/index.md` must be updated to reflect all current pages and headings.

---

## Module Wiki Index Template (`<module>/wiki/index.md`)

```markdown
# Module Wiki Table of Contents

> Quick discovery index for AI agents and developers.

## 1. Architectural Concepts (`type: concept`)
- **[JWT Token Handling & Rotation Strategy](concepts/jwt-strategy.md)**
  - Context & Architectural Rationale
  - System Mechanics & High-Level Flow
    - Token Minting & Key Distribution
    - Emergency Revocation Blacklist
  - Known Constraints & Trade-offs
- **[OAuth2 Integration Flow](concepts/oauth2-flow.md)**
  - Provider Abstraction Model
  - Token Exchange & State Verification

## 2. Architecture & Design Records (`type: decision-record | architecture`)
- **[ADR-001: Redis for Token Blacklisting](decisions/adr-001-redis-blacklist.md)**
  - Problem Statement & Evaluation Criteria
  - Decision Outcome & Impact
```

---

## Global Repository Wiki Index Template (`wiki/index.md` at root)

```markdown
# Global Repository Wiki Index

> Repository-wide knowledge base index for architecture, cross-cutting concerns, and module wikis.

## 1. Global Architecture & Standards
- **[System Architecture Overview](architecture/overview.md)**
  - High-Level Service Topology
  - Ingress Router & API Gateway Pattern
  - Database Sharding & Transaction Model
- **[Coding Standards & Guidelines](guides/coding-standards.md)**
  - Error Handling & Logging Standards
  - Security & Input Sanitization Rules

## 2. Module Wiki Directory
- **[Auth Module Wiki](../auth-service/wiki/index.md)** — Token handling, OAuth2 workflows, security decisions.
- **[Billing Module Wiki](../billing-service/wiki/index.md)** — Payment processing, webhook handling, tax calculations.
```
