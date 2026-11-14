---
version: 2
name: cross-cutting-analytical-datastore-client-implementati-cf5e
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Analytical Datastore Client Implementations Encapsulate Remote"
metadata:
  actual:
    adr_id: cf5e9b93-1d6e-41da-bf66-cf2761e1b9d3
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Analytical Datastore Client Implementations Encapsulate Remote

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all data access operations querying remote analytical datastore pipes within the workspace and client module configurations managing datastore authentication credentials and endpoint URLs.

### Rules

- **R-ZOD-001** MUST: Analytical datastore client implementations MUST encapsulate remote pipe queries using the @chronark/zod-bird Client abstraction with explicit base URL configuration and authentication token resolution.

### Verify

```bash
# Discover workspace test execution script and run analytical datastore client unit tests
# Discover static type analysis script and verify type compatibility across all client schemas and queries
```

**Accept when:**
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and peer code reviews enforce client schema validation and error handling.
</enforcement>