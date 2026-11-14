---
version: 2
name: cross-cutting-shared-state-requiring-multi-request-fdd8
description: "Standard Web API and Built-in Map Accessors for Ephemeral State: Shared State Requiring Multi Request Caching"
metadata:
  actual:
    adr_id: fdd8c972-d375-435f-b0c8-302cd791540a
    status: accepted
    candidate_group_id: 08739b2f-55a4-46ce-b7d2-8b39f42ac4ce
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Standard Web API and Built-in Map Accessors for Ephemeral State: Shared State Requiring Multi Request Caching

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHARED-001** SHOULD: Shared state requiring multi-request caching, cross-process consistency, or distributed invalidation SHOULD be delegated to dedicated primary datastore infrastructure rather than in-memory process collections.

### Verify

```bash
# Discover and run repository static analysis and type check scripts
# Discover and execute test suite scripts
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes touching data access and state management layers must be verified via continuous integration pipelines executing automated static analysis and test validation suites.
</enforcement>