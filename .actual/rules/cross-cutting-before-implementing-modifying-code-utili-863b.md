---
version: 2
name: cross-cutting-before-implementing-modifying-code-utili-863b
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Before Implementing Modifying Code Utilizing Trpc"
metadata:
  actual:
    adr_id: 863b095d-36ed-4ca3-ab07-084f4eff6c50
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

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Before Implementing Modifying Code Utilizing Trpc

Scope: applies to `packages/api/index.ts`, `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Before implementing or modifying code utilizing @trpc/server, the consumer MUST inspect the repository lock artifact to determine the exact locked version and verify API compatibility against official version documentation.

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
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing raw unmanaged endpoints or non-standard procedure definitions will fail automated lint and type validation checks and will be blocked from merging.
</enforcement>