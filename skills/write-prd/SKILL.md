---
name: write-a-prd
description: Create a Product Requirements Document (PRD) through user interview, codebase exploration, and module design, then submit as a GitHub issue. Use when user wants to write a PRD, create a product requirements document, plan a new feature, or define product requirements.
---

## Quick start

Ask the user: "Describe the problem you want to solve and any solution ideas you have."

## Workflow

- [ ] **Interview** — Get a long, detailed description of the problem and potential solutions
- [ ] **Explore** — Verify assertions by reading the codebase; understand current state
- [ ] **Deep dive** — Interview relentlessly: walk every branch of the design tree, resolve dependencies one-by-one
- [ ] **Module sketch** — Identify major modules to build or modify; prefer deep modules (rich functionality, simple stable interface, testable in isolation) over shallow ones; confirm with user which need tests
- [ ] **Write PRD** — Use the template in [REFERENCE.md](REFERENCE.md); submit as a GitHub issue

## Key principles

- Resolve decision dependencies before moving on (don't leave forks unresolved)
- Prefer deep modules: complex internals, simple interface, isolated testability
- Confirm module list and test coverage with the user before drafting

See [REFERENCE.md](REFERENCE.md) for the full PRD template.
