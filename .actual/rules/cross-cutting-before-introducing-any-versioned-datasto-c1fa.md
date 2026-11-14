---
version: 2
name: cross-cutting-before-introducing-any-versioned-datasto-c1fa
description: "Standard Web API and Built-in Map Accessors for Ephemeral State: Before Introducing Any Versioned Datastore External"
metadata:
  actual:
    adr_id: c1fa62a4-0773-48f2-b858-486c97cf066b
    status: accepted
    candidate_group_id: 08739b2f-55a4-46ce-b7d2-8b39f42ac4ce
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Standard Web API and Built-in Map Accessors for Ephemeral State: Before Introducing Any Versioned Datastore External

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for single-request lifecycle parameter extraction, header propagation, cookie inspection, localized dictionary lookups for UI component metric formatting, and impact aggregation routines.

### Rules

- **R-EPHEMERAL-001** MUST: Before introducing any versioned datastore or external caching client, developers MUST inspect the repository dependency manifest and lock artifact to verify the authoritative resolved version against official public documentation.
- **R-EPHEMERAL-002** MUST: Access request parameters, headers, cookies, and form payloads via standard platform request accessors directly at request handling boundaries.
- **R-EPHEMERAL-003** MUST: Scope temporary transformation dictionaries to individual function or component lifecycles to facilitate prompt garbage collection upon request completion.

### Verify

```bash
# Discover and run the repository static analysis and type check scripts
# Discover and execute test suite scripts to verify request parsing and data aggregation transformations
```

**Accept when:**
- Static analysis and type checking complete with zero errors across all request handling and metric transformation modules.
- Unit and integration test suites pass successfully without requiring external cache daemon or mock connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes touching data access and state management layers must be verified via continuous integration static analysis, test validation suites, and mandatory architectural peer code reviews.
</enforcement>