---
version: 2
name: cross-cutting-backend-datastore-access-components-use-61df
description: "sqlx Primary Datastore Library: Backend Datastore Access Components Use Sqlx"
metadata:
  actual:
    adr_id: 61dff0f9-9a68-4bc6-afac-e327afffffe6
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

# sqlx Primary Datastore Library: Backend Datastore Access Components Use Sqlx

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/tinybird/**`.

These rules are ALWAYS ACTIVE for relational primary datastore connection initialization and query lifecycle management within internal backend services.

### Rules

- **R-SQLX-001** MUST: Backend datastore access components MUST use sqlx as the primary relational datastore abstraction layer, wrapping standard datastore connection handles initialized via sql.Open.

### Verify

```bash
# Discover and run the project static analysis and linting verification suite to confirm datastore connections utilize approved constructor interfaces.
# Discover and execute the project unit and integration test suite to validate datastore connection initialization and cleanup routines.
```

**Accept when:**
- Static verification confirms all relational datastore access points use sqlx connection abstractions without unapproved database drivers.
- Integration tests confirm datastore initialization and teardown routines execute cleanly without resource leaks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks inspect datastore package dependencies and peer code reviews verify new datastore client implementations or configuration changes.
</enforcement>