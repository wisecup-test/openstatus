---
version: 2
name: cross-cutting-procedure-handlers-not-duplicate-cross-4a1a
description: "@trpc/server Procedure Middleware Composition: Procedure Handlers Not Duplicate Cross Cutting"
metadata:
  actual:
    adr_id: 4a1a10c6-b2ca-41ca-9135-bc7dcdcfc5ac
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Procedure Handlers Not Duplicate Cross Cutting

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST_NOT: Procedure handlers MUST NOT duplicate cross-cutting duration calculations or identity verification logic internally within individual endpoint resolvers.

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