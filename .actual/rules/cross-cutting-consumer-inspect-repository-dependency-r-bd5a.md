---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-r-bd5a
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Consumer Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: bd5a701a-bde1-494c-91d3-f2cceb594fac
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Consumer Inspect Repository Dependency Resolution Artifact

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: The consumer MUST inspect the repository dependency resolution artifact to determine the exact locked version of the client and schema validation libraries before authoring datastore integration code.
- **R-ZOD-002** MUST: Configure client instances with explicit environment token resolution and default base URL fallbacks.
- **R-ZOD-003** MUST: Define reusable schema objects for shared query parameters including periods, timestamps, and record counters.

### Verify

```bash
# Discover and execute workspace analytical datastore client unit tests from the repository manifest
# Discover and execute the static type analysis script from the repository configuration
```

**Accept when:**
- Analytical client test suites pass with zero validation errors.
- Static type analysis confirms all pipe query parameters satisfy defined schema contracts.
- Client initialization safely handles missing authentication tokens without unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests bypassing the central client or omitting schema validation are blocked from merging.
</enforcement>