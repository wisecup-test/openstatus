---
version: 2
name: cross-cutting-developers-not-treat-ephemeral-memory-11da
description: "Standard Web API and Built-in Map Accessors for Ephemeral State: Developers Not Treat Ephemeral Memory Map"
metadata:
  actual:
    adr_id: 11da8dbd-eaac-4352-b5a7-2d852052fb7e
    status: accepted
    candidate_group_id: 08739b2f-55a4-46ce-b7d2-8b39f42ac4ce
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Standard Web API and Built-in Map Accessors for Ephemeral State: Developers Not Treat Ephemeral Memory Map

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EPHEMERAL-001** MUST_NOT: Developers MUST NOT treat ephemeral in-memory Map instances or request-bound property accessors as persistent or shared cache layers across independent execution lifecycles.

### Verify

```bash
# Discover and run the repository static analysis and type check scripts to verify correct property access on platform request structures.
# Discover and execute test suite scripts to verify that request parsing and data aggregation transformations pass all assertions without external cache dependencies.
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>