# Review Dimensions — Detailed Criteria

## Architecture

- Is the change in the right layer/module for what it does?
- Does it introduce a new pattern where an established one already exists?
- Are dependency directions correct (no circular deps, no lower layer importing higher)?
- If a new abstraction is introduced, is it justified by more than one call site?
- Does it respect existing boundaries (e.g., domain logic not leaking into controllers)?

## Conventions

- Do names (files, functions, variables, classes) follow the patterns in neighboring code?
- Is error handling consistent with how sibling modules handle errors?
- Does logging match the level/format used elsewhere?
- Are new files placed in the expected directory per existing structure?
- Do imports follow the project's ordering/grouping convention?

## Integration safety

- If a shared interface/type changed, are ALL consumers updated?
- If behavior changed (return type, error shape, side effects), are callers aware?
- Could this silently change behavior for existing users of the API/function?
- Are database migrations reversible? Do they need a backfill?
- If a feature flag is involved, is the off-state safe?

## Tests

- Is there test coverage for the new/changed behavior?
- Do test patterns match how comparable features are tested in this repo?
- Are edge cases covered (empty input, null, boundary values, error paths)?
- If tests were removed or modified, is the removed coverage still needed?
- Do tests rely on implementation details that will break on refactor?

## Hidden risks

- **Race conditions**: shared mutable state, concurrent access without locking?
- **N+1 queries**: loop that triggers a query per iteration?
- **Auth/authz boundaries**: does new code respect permission checks?
- **Missing migrations**: schema change without a migration file?
- **Backward compat**: will this break existing clients, APIs, or stored data?
- **Resource leaks**: opened connections/files/handles that aren't closed?
- **Secrets**: hardcoded credentials, tokens, or keys?

## CI status

- Are all required checks passing?
- If a check is failing, is the failure related to this PR's changes?
- Are there new warnings in CI output that weren't there before?
- If CI is skipped or pending, flag it — the user should know.
