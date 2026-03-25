# Reference

## Dependency Categories

When assessing a candidate for deepening, classify its dependencies:

### 1. In-process

Pure computation, in-memory state, no I/O. Always deepenable — just merge the modules and test directly.

### 2. Local-substitutable

Dependencies that have local test stand-ins (e.g., PGLite for Postgres, in-memory filesystem). Deepenable if the test substitute exists. The deepened module is tested with the local stand-in running in the test suite.

### 3. Remote but owned (Ports & Adapters)

Your own services across a network boundary (microservices, internal APIs). Define a port (interface) at the module boundary. The deep module owns the logic; the transport is injected. Tests use an in-memory adapter. Production uses the real HTTP/gRPC/queue adapter.

Recommendation shape: "Define a shared interface (port), implement an HTTP adapter for production and an in-memory adapter for testing, so the logic can be tested as one deep module even though it's deployed across a network boundary."

### 4. True external (Mock)

Third-party services (Stripe, Twilio, etc.) you don't control. Mock at the boundary. The deepened module takes the external dependency as an injected port, and tests provide a mock implementation.

## Testing Strategy

The core principle: **replace, don't layer.**

- Old unit tests on shallow modules are waste once boundary tests exist — delete them
- Write new tests at the deepened module's interface boundary
- Tests assert on observable outcomes through the public interface, not internal state
- Tests should survive internal refactors — they describe behavior, not implementation

## Issue Template

<issue-template>

## Problem

Describe the technical debt:

- **Debt category**: (shallow module / dead code / missing error handling / inconsistent patterns / missing DI / argument overloading / SOLID violation / missing pure functions / test coverage hole / missing transactions)
- Which files/modules are affected
- What breaks, what's at risk, or what becomes hard because of this
- Why impact is high

## Proposed Solution

The chosen solution design:

- What changes (interface, pattern, structure)
- Usage example showing how callers or tests interact after the change
- What complexity or risk it eliminates

## Dependency Strategy

Which category applies and how dependencies are handled (if relevant):

- **In-process**: merged directly
- **Local-substitutable**: tested with [specific stand-in]
- **Ports & adapters**: port definition, production adapter, test adapter
- **Mock**: mock boundary for external services

## Testing Strategy

- **New tests to write**: describe the behaviors to verify at the boundary
- **Old tests to delete**: list tests that become redundant or were testing implementation detail
- **Test environment needs**: any local stand-ins or adapters required

## Implementation Recommendations

Durable guidance NOT coupled to current file paths:

- What the module/function should own (responsibilities)
- What it should hide (implementation details)
- What it should expose (the contract)
- How callers should migrate to the new approach

</issue-template>
