---
version: 2
name: cross-cutting-remote-procedure-definitions-build-upon-ccae
description: "@trpc/server Procedure Middleware Composition: Remote Procedure Definitions Build Upon Base"
metadata:
  actual:
    adr_id: ccae1c9f-1193-44f2-a48e-1cbabc94c2a8
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Remote Procedure Definitions Build Upon Base

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: All remote procedure definitions MUST build upon base procedure builders composed via @trpc/server procedure middleware using the use method.

### Verify

```bash
# Discover and execute type-checking task configured in the project workspace
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