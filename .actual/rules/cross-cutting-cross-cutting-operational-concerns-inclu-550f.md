---
version: 2
name: cross-cutting-cross-cutting-operational-concerns-inclu-550f
description: "@trpc/server API Service Boundary Definition: Cross Cutting Operational Concerns Including Timing"
metadata:
  actual:
    adr_id: 550fde38-f323-4ec0-b59d-39124f71c69d
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Cross Cutting Operational Concerns Including Timing

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Cross-cutting operational concerns including timing telemetry and authentication enforcement MUST be implemented as reusable procedure middleware.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration type checking and lint rules verify procedure definitions and schema bindings.
</enforcement>