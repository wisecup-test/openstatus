---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-d611
description: "@trpc/server Procedure Middleware Composition: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: d6111b51-50eb-4ea3-846b-039d435ec52c
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all remote procedure definitions, router compositions exposed through @trpc/server, and cross-cutting boundary concerns including request execution timing, operational telemetry, and authentication guards.

### Rules

- **R-TRPC-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved version of @trpc/server and verify that all procedure and middleware APIs exist within that exact version documentation prior to implementation.
- **R-TRPC-002** MUST: Middleware functions must handle downstream execution asynchronously and compute total duration upon promise resolution.
- **R-TRPC-003** MUST: Context enrichment must return a new context object containing validated identity records rather than mutating existing request state.

### Verify

```bash
# Discover and execute the repository type-checking task configured in the project workspace
# Discover and run the test suite covering API procedure definitions and middleware execution pipelines
# Discover and execute the repository static analysis suite to confirm compliance with procedure declaration conventions
```

**Accept when:**
- Type checking completes without diagnostic errors across all procedure builder chains and downstream context references.
- Middleware tests verify that duration logging and authorization guards execute in prescribed sequence for procedure invocations.
- Static analysis passes with zero violations regarding omitted middleware on protected procedure declarations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All procedure definitions exposed through @trpc/server must be verified using the repository type-checking, test, and static analysis tasks.
</enforcement>