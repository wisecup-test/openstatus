---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-2802
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Developers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: 2802f6ff-de43-4ccb-8a9b-5d635d70a06c
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Developers Inspect Repository Dependency Manifest Resolution

Scope: applies to `apps/server/src/routes/rpc/handlers/status-page/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** MUST: Developers MUST inspect the repository dependency manifest and resolution lock artifact to verify the exact resolved version of the contract dependency before authoring or modifying contract integrations.

### Verify

```bash
# Discover and execute the test runner covering status page remote procedure call handlers and converters
# Discover and run the project static type checker
# Discover and execute repository linting and formatting scripts
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status page remote procedure call handlers and data converters must strictly adhere to the resolved contract package definitions and be verified by CI pipelines, static analysis, and code reviews.
</enforcement>