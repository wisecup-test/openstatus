---
version: 2
name: cross-cutting-client-configuration-support-parsed-noop-3d15
description: "@chronark/zod-bird Client Adoption for Analytical Datastore Queries: Client Configuration Support Parsed Noop Execution"
metadata:
  actual:
    adr_id: 3d152fc3-1659-4ebf-a0d9-671d27239b7d
    status: accepted
    candidate_group_id: 36d83bce-4905-419a-8a43-990db2b384da
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - src/**
---

# @chronark/zod-bird Client Adoption for Analytical Datastore Queries: Client Configuration Support Parsed Noop Execution

Scope: applies to `packages/tinybird/src/**`.

These rules are ALWAYS ACTIVE for all data access operations querying remote analytical datastore pipes within the workspace and client module configurations managing datastore authentication credentials and endpoint URLs.

### Rules

- **R-ZOD-001** SHOULD: The client configuration SHOULD support a parsed noop execution flag to facilitate dry-run operations and isolated testing without contacting remote endpoints.
- **R-ZOD-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ZOD-003** MANDATORY: Before writing code that uses a versioned library, the consumer MUST execute in order: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API exists in that version, and re-run as needed per dependency at point of use.
- **R-ZOD-004** MANDATORY: Configure client instances with explicit environment token resolution and default base URL fallbacks.
- **R-ZOD-005** MANDATORY: Define reusable schema objects for shared query parameters including periods, timestamps, and record counters.

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
Claude Code MUST NOT skip or defer verification. Continuous integration test suites and peer code reviews verify client schema validation and error handling. Pull requests bypassing the central client or omitting schema validation are blocked from merging.
</enforcement>