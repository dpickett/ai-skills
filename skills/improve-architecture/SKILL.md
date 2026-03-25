---
name: improve-codebase-architecture
description: Explore a codebase to surface and prioritize technical debt, then design solutions and file issues. Use when user wants to improve architecture, flag technical debt, find refactoring opportunities, improve testability, or identify SOLID violations, missing DI, dead code, coverage holes, over-abstraction, inconsistent patterns, missing error handling, argument overloading, or missing database transactions.
---

# Improve Codebase Architecture

Explore a codebase, surface technical debt by impact, and file well-documented issues for follow-up work.

Intellectual framework: **deep modules** (Ousterhout), **SOLID principles**, **dependency injection**, and **domain-driven design** are all first-class lenses. They complement each other — apply whichever is most relevant to the debt at hand.

## Process

### 1. Explore the codebase

Use the Agent tool with subagent_type=Explore to navigate the codebase organically. Look across all debt categories simultaneously. Note where you experience friction — the friction IS the signal.

Debt categories to watch for:

- **Shallow modules / over-abstraction**: Interface nearly as complex as the implementation, or unnecessary indirection hiding nothing
- **Dead code**: Unreferenced exports, unreachable branches, unused dependencies
- **Missing or weak error handling**: Swallowed exceptions, unchecked nulls, silent failures
- **Inconsistent patterns**: Same problem solved differently in different parts of the codebase
- **Missing dependency injection**: Hard-coded dependencies that prevent testing or substitution
- **Argument overloading**: Functions with too many parameters, boolean flags, or implicit coupling through shared objects
- **SOLID violations**: Single-responsibility breaches, open/closed violations, Liskov substitutions, interface segregation failures, dependency inversions
- **Missing pure functions**: Side effects embedded in logic that could be pure — real bugs hiding in how pure-ish functions are called
- **Test coverage holes**: Untested critical paths, tests that only cover the happy path, tests that assert on implementation rather than behavior
- **Missing database transactions**: Multiple writes that should be atomic but aren't

### 2. Prioritize by impact and present the menu

Rank findings by **impact** — data loss risk, blast radius, likelihood of production failure, difficulty of future change. Present a numbered list grouped by debt category. For each item:

- **Location**: Files/modules involved
- **Debt category**: Which category above applies
- **Impact**: Why this matters — what breaks, what's at risk, what becomes hard
- **Effort**: Rough signal (small / medium / large)

Ask: "Which of these would you like to address?"

### 3. User picks a candidate

### 4. Frame the problem space

Before spawning sub-agents, write a user-facing explanation of the problem:

- What's wrong and why it matters
- The constraints any solution must satisfy
- Dependencies involved (see [REFERENCE.md](REFERENCE.md) for dependency categories)
- A rough illustrative sketch (code or pseudo-code) to make the problem concrete — not a proposal, just grounding

Show this to the user, then immediately proceed to Step 5. The user reads while sub-agents work.

### 5. Design solutions

**For dead code only**: Skip sub-agents. Write a single issue listing what to remove and why. Proceed to Step 7.

**For all other categories**: Spawn 3 sub-agents in parallel using the Agent tool. Each must produce a **radically different** solution. Give each a technical brief (file paths, debt details, dependency category) and a distinct design constraint:

- Agent 1: "Minimize the change — the smallest fix that meaningfully improves this"
- Agent 2: "Maximize correctness — solve this completely, even if it's a larger refactor"
- Agent 3: "Design for future extensibility — solve this in a way that makes the next change easy"

Each sub-agent outputs:

1. Proposed solution (interface, pattern, or structural change)
2. Usage example showing how callers or tests interact with it after the change
3. What complexity it hides or eliminates
4. Dependency strategy if applicable (see [REFERENCE.md](REFERENCE.md))
5. Trade-offs

Present designs sequentially, then compare in prose. Give your own recommendation — which is strongest and why. If a hybrid is stronger, propose it. Be opinionated.

### 6. User picks a solution (or accepts recommendation)

### 7. Create issue

If the Shortcut MCP is available and configured — create a chore in Shortcut documenting the problem and chosen solution using the issue template in [REFERENCE.md](REFERENCE.md).

If not — write the issue to `digital-brain/architectural-improvements/` from the root of the repository, using the same template.

The issue must be self-contained: a subsequent agent with no conversation context should be able to pick it up and implement the solution.
