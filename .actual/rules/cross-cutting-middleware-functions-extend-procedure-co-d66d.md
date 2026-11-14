---
version: 2
name: cross-cutting-middleware-functions-extend-procedure-co-d66d
description: "@trpc/server Procedure Middleware Composition: Middleware Functions Extend Procedure Context Immutably"
metadata:
  actual:
    adr_id: d66de4d0-d4fb-4f90-b6e4-20590d0c7892
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Middleware Functions Extend Procedure Context Immutably

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all remote procedure definitions and router compositions exposed through @trpc/server involving cross-cutting boundary concerns including request execution timing, operational telemetry, and authentication guards.

### Rules

- **R-TRPC-001** SHOULD: Middleware functions SHOULD extend procedure context immutably, forwarding enriched identity and telemetry metadata downstream to subsequent middleware and handlers.

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
Claude Code MUST NOT skip or defer verification. All pull requests exposing procedures without required middleware chains are blocked from merging.
</enforcement>