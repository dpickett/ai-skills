# Quality Tool Reference

## Nx Monorepo

When `nx.json` is present, Nx targets take priority over direct tool invocations.

| Task | Command |
|------|---------|
| Lint a project | `pnpm exec nx run <project>:lint` |
| Test a project | `pnpm exec nx run <project>:test` |
| Type check a project | `pnpm exec nx run <project>:typecheck` |
| Build a project | `pnpm exec nx run <project>:build` |
| All affected (lint) | `pnpm exec nx affected --target=lint` |
| All affected (test) | `pnpm exec nx affected --target=test` |
| Show project targets | `pnpm exec nx show project <project>` |
| List workspace projects | `pnpm exec nx show projects` |

To find which project owns a changed file:
```bash
pnpm exec nx show projects --affected --files=<comma-separated-paths>
```

## JavaScript / TypeScript

Detect the package manager by lock file: `pnpm-lock.yaml` → pnpm (default), `yarn.lock` → yarn, `package-lock.json` → npm.

| Tool | Detect | Format | Lint | Type check |
|------|--------|--------|------|------------|
| ESLint | `.eslintrc*`, `eslint.config.*` | — | `pnpm exec eslint --fix <files>` | — |
| Prettier | `.prettierrc*`, `prettier.config.*` | `pnpm exec prettier --write <files>` | — | — |
| TypeScript | `tsconfig.json` | — | — | `pnpm exec tsc --noEmit` |
| Biome | `biome.json` | `pnpm exec biome format --write <files>` | `pnpm exec biome lint --apply <files>` | — |
| pnpm scripts | `package.json` `.scripts` | `pnpm run format` | `pnpm run lint` | `pnpm run typecheck` |

Check `package.json` scripts first — the project may wrap any of the above under `lint`, `format`, `typecheck`, or `check`.

## Python

| Tool | Detect | Format | Lint | Type check |
|------|--------|--------|------|------------|
| Ruff | `ruff.toml`, `pyproject.toml [tool.ruff]` | `ruff format <files>` | `ruff check --fix <files>` | — |
| Black | `pyproject.toml [tool.black]`, `.black` | `black <files>` | — | — |
| Flake8 | `.flake8`, `setup.cfg [flake8]` | — | `flake8 <files>` | — |
| mypy | `mypy.ini`, `pyproject.toml [tool.mypy]` | — | — | `mypy <files>` |
| pyright | `pyrightconfig.json` | — | — | `pyright <files>` |

## Ruby

| Tool | Detect | Format + Lint |
|------|--------|---------------|
| RuboCop | `.rubocop.yml` | `rubocop -a <files>` |
| Standard | `.standard.yml` | `standardrb --fix <files>` |

## Rust

| Tool | Detect | Format | Lint |
|------|--------|--------|------|
| rustfmt | `Cargo.toml` | `cargo fmt` | — |
| Clippy | `Cargo.toml` | — | `cargo clippy --fix` |

## Go

| Tool | Detect | Format | Lint |
|------|--------|--------|------|
| gofmt | `go.mod` | `gofmt -w <files>` | — |
| golangci-lint | `.golangci.yml` | — | `golangci-lint run` |

## Make / generic

If a `Makefile` is present, check for targets:

```bash
make lint
make format
make typecheck
make test
make check   # often runs all of the above
```

## Test runners

| Ecosystem | Command |
|-----------|---------|
| Node (Jest) | `pnpm exec jest <file>` |
| Node (Vitest) | `pnpm exec vitest run <file>` |
| Node (Playwright) | `pnpm exec playwright test` |
| Python (pytest) | `pytest <file>` |
| Ruby (RSpec) | `bundle exec rspec <file>` |
| Rust | `cargo test` |
| Go | `go test ./...` |

Prefer running the subset of tests covering changed files rather than the full suite unless the project is small or you suspect wider impact.
