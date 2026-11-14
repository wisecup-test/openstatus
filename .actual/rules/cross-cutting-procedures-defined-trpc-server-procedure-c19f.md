---
version: 2
name: cross-cutting-procedures-defined-trpc-server-procedure-c19f
description: "@trpc/server API Service Boundary Definition: Procedures Defined Trpc Server Procedure Builders"
metadata:
  actual:
    adr_id: c19f225f-a305-468c-b3a4-57c36b5d9e40
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Procedures Defined Trpc Server Procedure Builders

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API router modules, procedure definitions, and middleware chains defined within the API boundary package.

### Rules

- **R-TRPC-001** MUST: API procedures MUST be defined using @trpc/server procedure builders to establish formal service and boundary contracts.

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