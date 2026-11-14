---
version: 2
name: cross-cutting-operational-telemetry-execution-duration-80a9
description: "@trpc/server Procedure Middleware Composition: Operational Telemetry Execution Duration Tracking Encapsulated"
metadata:
  actual:
    adr_id: 80a97ac5-0d1d-457b-be16-962a09cde993
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Operational Telemetry Execution Duration Tracking Encapsulated

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all remote procedure definitions, router compositions exposed through @trpc/server, and cross-cutting boundary concerns including request execution timing, operational telemetry, and authentication guards.

### Rules

- **R-TRPC-001** MUST: Operational telemetry and execution duration tracking MUST be encapsulated within dedicated timing middleware attached to base procedure declarations.
- **R-TRPC-002** MUST: Middleware functions must handle downstream execution asynchronously and compute total duration upon promise resolution.
- **R-TRPC-003** MUST: Context enrichment must return a new context object containing validated identity records rather than mutating existing request state.

### Verify

```bash
# Discover and execute the repository type-checking task configured in the project workspace
# Discover and run the test suite covering API procedure definitions and middleware execution pipelines
# Discover and execute the repository static analysis suite
```

**Accept when:**
- Type checking completes without diagnostic errors across all procedure builder chains and downstream context references.
- Middleware tests verify that duration logging and authorization guards execute in prescribed sequence for procedure invocations.
- Static analysis passes with zero violations regarding omitted middleware on protected procedure declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>