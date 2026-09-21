# AGENTS.md

## Repository purpose

This repository is the architecture hub for the LifeTrace ecosystem.

It is intentionally **documentation-only**. Do not treat it as a runnable application repository.

## Rules for AI agents and contributors

1. Do not add production application code to this repository.
2. Do not recreate the old Monorepo structure such as `apps/`, `services/`, `crates/`, `contracts/`, or `deploy/`.
3. Do not infer current product behavior from Git history unless the task explicitly asks for historical analysis.
4. For implementation questions, inspect the authoritative product repository:
   - `LifeTrace-execute`
   - `LifeTrace-finance`
   - `LifeTrace-assets`
   - `LifeTrace-desktop`
   - `LifeTrace-web`
   - `LifeTrace-cloud`
   - `LifeTrace-agent`
5. Keep only cross-repository architecture, ownership boundaries, contracts at the conceptual level, and ADRs here.
6. A system-boundary change requires an ADR before implementation in child repositories.
7. Product-specific implementation plans belong in that product repository, preferably under its OpenSpec process.
8. Never copy a child repository's source code into this repository for reference. Link to the authoritative repository instead.
9. When architecture documentation and product implementation disagree, identify the mismatch explicitly. Do not silently rewrite ownership boundaries.
10. Preserve the principle: one authoritative owner per domain entity, with cross-app access through explicit contracts, APIs, EntityLink, or Agent capabilities.

## Allowed file types

Primarily Markdown and architecture-diagram source embedded in documentation.

Repository-level metadata is acceptable only when needed to maintain this documentation repository.
