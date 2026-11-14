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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Authentication and identity verification MUST be enforced via sequential middleware chaining before procedure handlers access protected context data.
- **R-TRPC-002** MANDATORY: Execute the discovery policy to identify tool names, file names, commands, package managers, and version numbers from the repository.
- **R-TRPC-003** MANDATORY: Ground lock versions by finding the dependency manifest, identifying the build tool, inspecting the lock or resolution artifact, looking up official docs for that exact version, confirming API existence, and repeating per dependency at point of use.
- **R-TRPC-004** MUST: Middleware functions must handle downstream execution asynchronously and compute total duration upon promise resolution.
- **R-TRPC-005** MUST: Context enrichment must return a new context object containing validated identity records rather than mutating existing request state.

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