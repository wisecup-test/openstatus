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
- **R-EPHEMERAL-002** MANDATORY: Execute lock-version grounding in order (manifest discovery, build tool identification, lock file inspection, version-specific documentation lookup, and API existence confirmation) before writing code that uses a versioned library.
- **R-EPHEMERAL-003** MANDATORY: Access request parameters, headers, cookies, and form payloads via standard platform request accessors directly at request handling boundaries.
- **R-EPHEMERAL-004** MANDATORY: Scope temporary transformation dictionaries to individual function or component lifecycles to facilitate prompt garbage collection upon request completion.

### Verify

```bash
# Discover and run repository static analysis and type check scripts
# Discover and execute test suite scripts to verify request parsing and data aggregation transformations pass without external cache dependencies
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes touching data access and state management layers must undergo automated static analysis, test validation, and mandatory architectural and peer code review.
</enforcement>