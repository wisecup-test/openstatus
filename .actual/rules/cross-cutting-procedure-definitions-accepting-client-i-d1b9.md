---
version: 2
name: cross-cutting-procedure-definitions-accepting-client-i-d1b9
description: "@trpc/server API Service Boundary Definition: Procedure Definitions Accepting Client Input Attach"
metadata:
  actual:
    adr_id: d1b9b8db-c4f8-41b8-bf4b-41b82357bf08
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Procedure Definitions Accepting Client Input Attach

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API router modules, procedure definitions, and middleware chains defined within the API boundary package.

### Rules

- **R-TRPC-001** MUST: All procedure definitions accepting client input MUST attach explicit input validation schemas using the input builder method.

### Verify

```bash
# Discover and execute project test suite for API procedures
npm test
# Discover and run static type checking across the API package
npm run typecheck
```

**Accept when:**
- All API procedures verify compile-time type safety across input, output, and context boundaries.
- Static analysis confirms schema validation is attached to all externally reachable procedure inputs.
- Middleware execution pipelines enforce timing and authentication constraints across all procedure invocations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>