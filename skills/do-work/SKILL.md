---
name: do-work
description: Plan, implement, validate, and commit a piece of work end-to-end. Use when the user asks you to implement a feature, fix a bug, or complete a task and wants a structured plan-then-build loop with quality gates and a clean commit.
---

# Do Work

End-to-end workflow: plan → implement → validate → commit.

## Step 0 — Understand the task

Restate the goal in one sentence. If ambiguous, ask one clarifying question before proceeding. Do not ask multiple questions — pick the most important one.

## Step 1 — Introspect quality tooling

While waiting or before starting, detect what quality gates are available. Run these in parallel:

```bash
# Monorepo / workspace detection
ls nx.json pnpm-workspace.yaml lerna.json turbo.json rush.json 2>/dev/null || true
cat nx.json 2>/dev/null | jq '{affected: .affected, defaultProject: .defaultProject}' || true

# Package manager / scripts
cat package.json 2>/dev/null | jq '.scripts' || true
cat Makefile 2>/dev/null | grep -E '^(lint|format|typecheck|test|check):' || true
cat pyproject.toml 2>/dev/null | grep -A2 '\[tool\.' || true
cat Cargo.toml 2>/dev/null | grep -A2 '\[package\]' || true

# Common tool configs
ls .eslintrc* .eslintignore eslint.config.* \
   .prettierrc* prettier.config.* \
   tsconfig*.json \
   .rubocop.yml \
   ruff.toml .flake8 mypy.ini \
   biome.json \
   2>/dev/null || true
```

**Monorepo rules:**
- If `nx.json` is present, prefer `nx` targets over direct tool invocations. Use `pnpm exec nx run <project>:lint`, `pnpm exec nx run <project>:test`, etc. — or `pnpm exec nx affected --target=<target>` when validating changes across the workspace.
- Identify the affected project(s) for the changed files before running checks. Use `pnpm exec nx show project <project>` to inspect available targets.
- If `turbo.json` is present, prefer `pnpm exec turbo run lint test typecheck` filtered to affected packages.
- For other monorepos (lerna, pnpm workspaces without nx/turbo), run checks scoped to the workspace package(s) containing changed files.

## Step 1 — Plan

Before writing any code:

1. Read every file you intend to change in full.
2. Read sibling files and shared types/interfaces to understand conventions.
3. Ask any questions, one at a time, until you're confident enough to put forth a plan
4. Write a short plan (bullet list) covering:
   - What will change and why
   - Which files will be created or modified
   - Any risks or constraints
5. Present the plan to the user and wait for approval before implementing.

Record which of these are present. See [REFERENCE.md](REFERENCE.md) for how to invoke each tool.

## Step 3 — Implement

Follow the approved plan. Apply these constraints:

- Only change what the plan describes — no opportunistic refactors.
- Match naming, error handling, and structure of neighboring code.
- Do not add comments, docstrings, or type annotations to code you did not touch.
- Be spare in your code comments and favor expressive code in favor of clarifying comments
- No speculative abstractions — solve the task, not hypothetical future tasks.

### TDD loop (backend code and frontend hooks/classes)

For backend code and frontend hooks or classes (not UI components), use a strict red → green → refactor loop, **one test at a time** in tracer-bullet style.

Repeat from until the planned behavior is fully covered.

**Tracer-bullet discipline:**
- Pick the thinnest end-to-end slice first (happy path), then layer in edge cases and error paths.
- Do not write multiple tests before going green. One failing test at a time.
- If a test is hard to write, that is a design signal — reconsider the interface before forcing it.

**TDD scope:**
- Apply to: backend services, controllers, utilities, data-access layers, frontend hooks, frontend classes/models.
- Skip for: React/Vue/Svelte UI components, markup-heavy templates, CSS, config files, scripts.

## Step 4 — Validate

Run every quality gate detected in Step 2 that applies to the changed files. Run independent checks in parallel where possible.

Typical sequence (adapt to what exists):

1. **Format** — fix formatting issues automatically if the tool supports it.
2. **Lint** — fix auto-fixable issues; flag anything requiring manual attention.
3. **Type check** — resolve all type errors before proceeding.
4. **Tests** — run the test suite (or the subset covering changed files). If tests fail, diagnose and fix before moving on.

If a check fails and you cannot resolve it, surface the failure to the user with the error output and your diagnosis. Do not commit with failing checks.

## Step 5 — Commit

Once all checks pass, stage only the files you intentionally changed and commit with a well-structured message:

```
<type>(<scope>): <short imperative summary under 72 chars>

<optional markdown formatted body: why this change was made, not what — the diff shows what>
```

**Type values:** `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `perf`

**Rules:**
- Use imperative mood: "add X", not "added X" or "adds X"
- Body only when the *why* is non-obvious
- Never use `git add -A` or `git add .` — stage files by name
- Never skip hooks (`--no-verify`)
- Never amend a published commit — create a new one

After committing, confirm success with `git log --oneline -3`.

## Checklist

- [ ] Task understood and confirmed
- [ ] Quality tooling detected
- [ ] Plan approved by user
- [ ] Implementation matches plan only
- [ ] TDD loop used for backend code and frontend hooks/classes (one test at a time)
- [ ] All available checks pass
- [ ] Only intended files staged
- [ ] Commit message follows convention
- [ ] Commit confirmed in log

See [REFERENCE.md](REFERENCE.md) for quality tool invocation commands.
