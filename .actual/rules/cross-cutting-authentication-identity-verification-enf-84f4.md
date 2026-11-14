---
version: 2
name: cross-cutting-authentication-identity-verification-enf-84f4
description: "@trpc/server Procedure Middleware Composition: Authentication Identity Verification Enforced Via Sequential"
metadata:
  actual:
    adr_id: 84f49ad5-ccd3-4cec-9807-affd65cd0d09
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Authentication Identity Verification Enforced Via Sequential

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all remote procedure definitions, router compositions, and cross-cutting boundary concerns exposed through @trpc/server.

### Rules

- **R-TRPC-001** MUST: Authentication and identity verification MUST be enforced via sequential middleware chaining before procedure handlers access protected context data.
- **R-TRPC-002** MANDATORY: Execute repository discovery policy to derive build tools, dependency manifests, lock files, and version-specific API references before writing code that uses a versioned library.
- **R-TRPC-003** MUST: Middleware functions must handle downstream execution asynchronously and compute total duration upon promise resolution.
- **R-TRPC-004** MUST: Context enrichment must return a new context object containing validated identity records rather than mutating existing request state.

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
Claude Code MUST NOT skip or defer verification. Pull requests exposing procedures without required middleware chains are blocked from merging.
</enforcement>