---
version: 2
name: cross-cutting-status-page-remote-procedure-call-cd1b
description: "Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Status Page Remote Procedure Call Service"
metadata:
  actual:
    adr_id: cd1b2185-e9a8-414c-aec9-d6e09dfb8309
    status: accepted
    candidate_group_id: 5905ec44-eb02-4bd6-b2b2-575b7f1fb0f4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/status-page/**
---

# Adoption of @openstatus/proto/status_page/v1 for Status Page RPC Interface Contracts: Status Page Remote Procedure Call Service

Scope: applies to `apps/server/src/routes/rpc/handlers/status-page/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SP-001** MUST: Status page remote procedure call service handlers and conversion layers MUST use @openstatus/proto/status_page/v1 contracts for all boundary payload definitions and wire message serialization.

### Verify

```bash
# Discover and execute the test suite covering status page remote procedure call handlers and converters
# Discover and run the project static type checker
# Discover and execute repository linting and formatting scripts
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>