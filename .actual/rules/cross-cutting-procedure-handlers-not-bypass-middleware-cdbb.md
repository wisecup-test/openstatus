---
version: 2
name: cross-cutting-procedure-handlers-not-bypass-middleware-cdbb
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedure Handlers Not Bypass Middleware Pipelines"
metadata:
  actual:
    adr_id: cdbbda20-eb3a-457a-a3eb-830bae45ca89
    status: accepted
    candidate_group_id: 339fa5ae-e0d7-4a41-b1e8-305ba484af35
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - index.ts
      - src/**
---

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedure Handlers Not Bypass Middleware Pipelines

Scope: applies to `packages/api/index.ts`, `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST_NOT: Procedure handlers MUST NOT bypass procedure middleware pipelines to perform bespoke authentication or unmanaged error serialization.

### Verify

```bash
# Discover and execute the project dependency and manifest audit scripts to verify lock-file synchronization for all server libraries.
# Discover and execute the project static type analysis and linting scripts to verify compliance of all procedure and router definitions.
# Discover and execute the project automated test suite covering API procedure execution, middleware behavior, and error handling paths.
```

**Accept when:**
- All API router modules export procedures constructed using the adopted server procedure builders and middleware chains.
- All API errors originating within procedure handlers conform to standard TRPC error representations.
- Static analysis and build validation confirm zero untyped or disparate procedure declarations across the API package.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>