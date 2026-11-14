---
version: 2
name: cross-cutting-server-route-handlers-client-side-7b0a
description: "Standard Web API and Built-in Map Accessors for Ephemeral State: Server Route Handlers Client Side View"
metadata:
  actual:
    adr_id: 7b0ab9ff-5c4b-49f2-bb1f-9a85b0670891
    status: accepted
    candidate_group_id: 08739b2f-55a4-46ce-b7d2-8b39f42ac4ce
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Standard Web API and Built-in Map Accessors for Ephemeral State: Server Route Handlers Client Side View

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EPHEMERAL-001** MAY: Server route handlers and client-side view components MAY construct local index structures for single-pass metric transformations and dictionary lookups.
- **R-EPHEMERAL-002** MANDATORY: Access request parameters, headers, cookies, and form payloads via standard platform request accessors directly at request handling boundaries.
- **R-EPHEMERAL-003** MANDATORY: Scope temporary transformation dictionaries to individual function or component lifecycles to facilitate prompt garbage collection upon request completion.
- **R-EPHEMERAL-004** MANDATORY: Discover the dependency manifest in the repo, identify the build tool, inspect the repository lock or resolution artifact for exact resolved versions, and look up official documentation before writing code using a versioned library.

### Verify

```bash
# Discover and run the repository static analysis and type check scripts
# Example: npm run lint / tsc

# Discover and execute test suite scripts
# Example: npm test
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines executing automated static analysis and test validation suites.
</enforcement>