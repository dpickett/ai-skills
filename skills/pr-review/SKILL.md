---
name: pr-review
description: Review a pull request and produce a filterable list of concerns. Use when user wants to review a PR, asks about PR quality, or references a pull request URL/number. Uses gh CLI to gather PR context. Does NOT post comments — outputs concerns for the user to triage.
---

You are performing a context-aware code review. Your output is a **private concerns list** for the user to triage. Do NOT post comments, approve, or request changes on the PR via `gh`. The user will decide what to surface.

## Step 0 — Identify the PR

If the user provided a PR URL or number, use it. Otherwise ask.

Extract the owner, repo, and PR number, then run all of these in parallel:

```bash
gh pr view <number> --json title,body,author,baseRefName,headRefName,state,additions,deletions,changedFiles,labels,reviews,reviewRequests,statusCheckRollup
gh pr diff <number>
gh pr checks <number>
gh api repos/{owner}/{repo}/pulls/<number>/files --paginate --jq '.[].filename'
gh api repos/{owner}/{repo}/pulls/<number>/comments --paginate
gh api repos/{owner}/{repo}/pulls/<number>/reviews --paginate
```

Note the PR description, existing review comments, and CI status — they inform your review.

## Step 1 — Read before you judge

Before writing any findings, actually read:

1. Every changed file in full (not just the diff hunks).
2. Sibling files in each directory containing a changed file.
3. Interfaces, types, or schemas the changed code imports or implements.
4. 2–3 existing features that do something analogous (your comparison baseline).
5. Adjacent test files and shared test utilities/fixtures.
6. Relevant config (linter, CI, ORM, dependency manifests).
7. `git log --oneline -20 -- <file>` for each changed file.

Do NOT skip this. Do NOT summarize what you "would" look at — actually read the files.

## Step 2 — Review dimensions

Evaluate each dimension. See [REFERENCE.md](REFERENCE.md) for detailed criteria.

- **Architecture** — right layer/module? new pattern where one exists? dependency direction?
- **Conventions** — naming, file structure, error handling, logging — match neighbors?
- **Integration safety** — shared interfaces changed? all consumers updated? implicit behavior shifts?
- **Tests** — coverage and patterns match comparable features? edge cases covered?
- **Hidden risks** — race conditions, N+1 queries, auth boundaries, missing migrations, backward compat?
- **CI status** — are checks passing? if not, is the failure related to this change?

## Step 3 — Deliver

Output the following sections. Do NOT post anything to GitHub.

### PR snapshot
| Field | Value |
|-------|-------|
| Title | from gh data |
| Author | from gh data |
| Base ← Head | base...head |
| Changed files | count |
| +/- | additions/deletions |
| CI status | pass/fail/pending |

### Concerns

For each concern, output a row. Group by severity, highest first.

| # | Severity | File:Line | Concern | Why it matters | Suggested fix |
|---|----------|-----------|---------|----------------|---------------|

Severity levels:
- **BLOCK** — must fix before merge (correctness, security, data loss)
- **WARN** — should fix, risk of subtle bugs or convention drift
- **NIT** — suggestion for clarity or consistency, safe to skip

### Positive callouts
List 1–3 things the PR does well — patterns worth reinforcing.

### Evidence trail
List every file and command you read/ran to form your review.

Skip style nitpicks a linter would catch. Focus on things that break, confuse, or silently diverge.
