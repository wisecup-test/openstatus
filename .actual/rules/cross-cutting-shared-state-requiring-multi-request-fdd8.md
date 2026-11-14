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
- **R-SHARED-002** MANDATORY: Access request parameters, headers, cookies, and form payloads via standard platform request accessors directly at request handling boundaries.
- **R-SHARED-003** MANDATORY: Scope temporary transformation dictionaries to individual function or component lifecycles to facilitate prompt garbage collection upon request completion.
- **R-SHARED-004** MANDATORY: Execute the lock-version grounding process before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist, and re-verify at point of use.

### Verify

```bash
# Discover and run repository static analysis and type check scripts
# Discover and execute test suite scripts
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration pipelines and peer code reviews.
</enforcement>