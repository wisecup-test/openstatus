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

These rules are ALWAYS ACTIVE for all status page remote procedure call service handlers and data conversion modules.

### Rules

- **R-SP-001** MUST: Status page remote procedure call service handlers and conversion layers MUST use @openstatus/proto/status_page/v1 contracts for all boundary payload definitions and wire message serialization.

### Verify

```bash
# Discover the repository test runner from the dependency manifest and execute the test suite covering status page remote procedure call handlers and converters.
# Discover the project static type checker and run type analysis to ensure strict adherence to protocol contract interfaces.
# Discover the repository linting and formatting scripts and execute them across the codebase to verify architectural boundary conformance.
```

**Accept when:**
- All status page remote procedure call handlers and data converters compile without type errors against the resolved contract package definitions.
- All automated test suites for status page endpoint handlers and data conversion modules pass successfully.
- No remote procedure call handler exposes database schema models directly across public response boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated CI pipelines, peer code reviews, and static analysis checking for unauthorized imports or direct database schema exposure across route handlers.
</enforcement>