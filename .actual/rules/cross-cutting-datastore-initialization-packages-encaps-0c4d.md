---
version: 2
name: cross-cutting-datastore-initialization-packages-encaps-0c4d
description: "sqlx Primary Datastore Library: Datastore Initialization Packages Encapsulate Connection Pool"
metadata:
  actual:
    adr_id: 0c4d1cfa-d806-4aa4-87e8-e6b3695c3fa0
    status: accepted
    candidate_group_id: 3af012f4-f5a3-46a8-bb9b-577ab0ca762b
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/database/**
      - internal/tinybird/**
---

# sqlx Primary Datastore Library: Datastore Initialization Packages Encapsulate Connection Pool

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/tinybird/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SQLX-001** MUST: Datastore initialization packages MUST encapsulate connection pool lifecycle management by exposing dedicated constructor and termination routines such as New and Close.

### Verify

```bash
# Discover and run the project static analysis and linting verification suite to confirm datastore connections utilize approved constructor interfaces.
# Discover and execute the project unit and integration test suite to validate datastore connection initialization and cleanup routines.
```

**Accept when:**
- Static verification confirms all relational datastore access points use sqlx connection abstractions without unapproved database drivers.
- Integration tests confirm datastore initialization and teardown routines execute cleanly without resource leaks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>