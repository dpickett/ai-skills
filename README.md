# AI Skills

An [OpenSkills](https://github.com/numman-ali/openskills) based set of skills that I use personally in my day-to-day AI assisted coding.

## Installation

Install dependencies:

```bash
pnpm install
```

Skills are managed via the `openskills` CLI. Install a skill from a GitHub repo or local path:

```bash
# From a GitHub repo
pnpm openskills install <github-url-or-repo>

# From a local path
pnpm openskills install ./skills/<skill-name>

# Install globally
pnpm openskills install -g <source>
```

List installed skills:

```bash
pnpm openskills list
```

Update all installed skills:

```bash
pnpm openskills update
```

## Skills

### architectural-interview

Conducts a structured architectural design interview, walking through design decisions one at a time until reaching a shared understanding and a concrete plan. If context can be gathered from the codebase, it does so automatically.

**Invoke with:** `/architectural-interview` in Claude Code

### pr-review

Reviews a pull request and produces a filterable list of concerns for you to triage. Uses the `gh` CLI to gather PR context (diff, checks, comments, reviews). Does **not** post comments or approve/request changes — the output is a private concerns list for you to act on.

**Invoke with:** `/pr-review` in Claude Code

### write-a-skill

Guides you through creating new agent skills with proper structure, progressive disclosure, and bundled resources. Gathers requirements, drafts the SKILL.md, and sets up any reference files or utility scripts needed.

**Invoke with:** `/write-a-skill` in Claude Code

### improve-architecture

Explores a codebase to surface and prioritize technical debt, then designs solutions and files issues for follow-up work. Applies deep modules (Ousterhout), SOLID principles, dependency injection, and domain-driven design as analytical lenses. Covers dead code, missing error handling, inconsistent patterns, missing DI, argument overloading, test coverage holes, missing database transactions, and more.

**Invoke with:** `/improve-architecture` in Claude Code
