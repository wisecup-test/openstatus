---
version: 2
name: cross-cutting-query-parameters-passed-analytical-pipe-3b4b
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Query Parameters Passed Analytical Pipe Endpoints"
metadata:
  actual:
    adr_id: 3b4b61f9-afe5-4533-8f36-c918be922e60
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Query Parameters Passed Analytical Pipe Endpoints

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all data access operations querying remote analytical datastore pipes within the workspace and client module configurations managing datastore authentication credentials and endpoint URLs.

### Rules

- **R-ZOD-001** MUST: All query parameters passed to analytical pipe endpoints MUST be parsed and validated against schema objects prior to request dispatch.

### Verify

```bash
# Discover and execute the workspace test execution script for analytical datastore client unit tests
# Discover and execute the static type analysis script across all datastore client schemas and queries
```

**Accept when:**
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and peer code reviews verify client schema validation and error handling.
</enforcement>