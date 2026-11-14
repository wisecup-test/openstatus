---
version: 2
name: cross-cutting-developers-isolate-transient-key-value-d157
description: "Standard Web API and Built-in Map Accessors for Ephemeral State: Developers Isolate Transient Key Value Lookups"
metadata:
  actual:
    adr_id: d15758a4-b89a-4147-9506-96ee459e3fbf
    status: accepted
    candidate_group_id: 08739b2f-55a4-46ce-b7d2-8b39f42ac4ce
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Standard Web API and Built-in Map Accessors for Ephemeral State: Developers Isolate Transient Key Value Lookups

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EPHEMERAL-001** MUST: Developers MUST isolate transient key-value lookups to language built-in Map collections and standard Web API request accessors for ephemeral, single-lifecycle data operations.

### Verify

```bash
# Discover and run repository static analysis and type check scripts
# Discover and execute test suite scripts
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>