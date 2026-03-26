---
name: prd-to-plan
description: Turn a PRD into a multi-phase software design document using tracer-bullet vertical slices, saved as a local Markdown file in ./digital-brain/software-design-documents/. Use when user wants to break down a PRD, create an implementation plan, plan phases from a PRD, or mentions "tracer bullets".
---

# PRD to Plan

Break a PRD into a phased implementation plan using vertical slices (tracer bullets). Output is a Markdown file in `./plans/`.

## Process

### 1. Confirm the PRD is in context

The PRD should already be in the conversation. If it isn't, ask the user to paste it or point you to the file.

### 2. Explore the codebase

If you have not already explored the codebase, do so to understand the current architecture, existing patterns, and integration layers.

### 3. Identify durable architectural decisions

Before slicing, identify high-level decisions that are unlikely to change throughout implementation:

- Route structures / URL patterns
- Database schema shape
- Key data models
- Authentication / authorization approach
- Third-party service boundaries

These go in the plan header so every phase can reference them.

### 4. Draft vertical slices

Break the PRD into **tracer bullet** phases. Each phase is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Do NOT include specific file names, function names, or implementation details that are likely to change as later phases are built
- DO include durable decisions: route paths, schema shapes, data model names
</vertical-slice-rules>

For each slice, write out headlines and subheadlines that effectively represent a user story. Add a paragraph to substantiate what the functionality accomplishes. We will use these to generate user stories in a subsequent task.

### 5. Quiz the user

Present the proposed breakdown as a numbered list.

- **Title**: short descriptive name
- **Objective**: goal of the slice

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Should any phases be merged or split further?

Iterate until the user approves the breakdown. Include implementation-agnostic acceptance criteria where appropriate.

### 6. Write the plan file

Create `./software-design-documents/` if it doesn't exist. Write the document as a Markdown file named after the feature (e.g. `./software-design-documents/user-onboarding.md`). Use the template below.

<plan-template>
# Plan: <Feature Name>

> Source PRD: <brief identifier or link>

## Architectural decisions

Durable decisions that apply across all phases:

- **Routes**: ...
- **Schema**: ...
- **Key models**: ...
- (add/remove sections as appropriate)

## Functional Outline

### Phase 1: <Title>

#### Function A

#### Function B

#### Function C

### Phase 2: <Title>

#### Function A

#### Function B

#### Function C

<!-- Repeat for each phase -->
</plan-template>
