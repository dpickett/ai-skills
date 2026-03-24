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
