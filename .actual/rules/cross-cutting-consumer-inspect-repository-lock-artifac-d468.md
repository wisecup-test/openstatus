---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-artifac-d468
description: "sqlx Primary Datastore Library: Consumer Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: d468f877-2e32-4f40-839b-c27c8807acd2
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

# sqlx Primary Datastore Library: Consumer Inspect Repository Lock Artifact Determine

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/tinybird/**`.

These rules are ALWAYS ACTIVE for relational primary datastore connection initialization and query lifecycle management within internal backend services.

### Rules

- **R-SQLX-001** MUST: The consumer MUST inspect the repository lock artifact to determine the exact resolved version of the sqlx dependency before implementing or extending datastore access code.

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