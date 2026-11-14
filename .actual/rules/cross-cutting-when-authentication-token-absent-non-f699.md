---
version: 2
name: cross-cutting-when-authentication-token-absent-non-f699
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: When Authentication Token Absent Non Production"
metadata:
  actual:
    adr_id: f69985cc-b985-4813-8961-eea9a14cfc65
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: When Authentication Token Absent Non Production

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all data access operations querying remote analytical datastore pipes within the workspace and client module configurations managing datastore authentication credentials and endpoint URLs.

### Rules

- **R-ZOD-001** SHOULD: When an authentication token is absent in non-production environments, the datastore client SHOULD log a descriptive warning and safely yield empty responses rather than throwing an unhandled runtime exception.

### Verify

```bash
# Discover and run workspace test execution script for analytical datastore client unit tests
# Discover and run static type analysis script to verify type compatibility across schemas and queries
```

**Accept when:**
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and peer code reviews verify client schema validation and error handling.
</enforcement>