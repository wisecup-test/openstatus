---
version: 2
name: cross-cutting-engineers-not-execute-raw-unvalidated-a92e
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Engineers Not Execute Raw Unvalidated Http"
metadata:
  actual:
    adr_id: a92e8411-3e40-4731-9c02-4565f04a497b
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Engineers Not Execute Raw Unvalidated Http

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all data access operations querying remote analytical datastore pipes within the workspace and client module configurations managing datastore authentication credentials and endpoint URLs.

### Rules

- **R-ZOD-001** MUST_NOT: Engineers MUST NOT execute raw, unvalidated HTTP calls directly to analytical pipe endpoints bypassing the central client interface.

### Verify

```bash
# Discover the workspace test execution script from the repository manifest and execute all analytical datastore client unit tests.
# Discover the static type analysis script from the repository configuration and verify type compatibility across all datastore client schemas and queries.
```

**Accept when:**
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests bypassing the central client or omitting schema validation are blocked from merging.
</enforcement>