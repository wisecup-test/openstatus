---
version: 2
name: cross-cutting-primary-datastore-connection-parameters-9255
description: "sqlx Primary Datastore Library: Primary Datastore Connection Parameters Credentials Retrieved"
metadata:
  actual:
    adr_id: 9255eed0-b01b-4deb-9fd4-1524b08411f9
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

# sqlx Primary Datastore Library: Primary Datastore Connection Parameters Credentials Retrieved

Scope: applies to `apps/private-location/internal/database/**`, `apps/private-location/internal/tinybird/**`.

These rules are ALWAYS ACTIVE for relational primary datastore connection initialization and query lifecycle management within internal backend services.

### Rules

- **R-DAT-001** MUST: Primary datastore connection parameters and credentials MUST be retrieved from runtime environment variables rather than embedded in source definitions.

### Verify

```bash
# Discover and run the project static analysis and linting verification suite to confirm datastore connections utilize approved constructor interfaces.
# Discover and execute the project unit and integration test suite to validate datastore connection initialization and cleanup routines.
```

**Accept when:**
- Static verification confirms all relational datastore access points use sqlx connection abstractions without unapproved database drivers.
- Integration tests confirm datastore initialization and teardown routines execute cleanly without resource leaks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks inspect datastore package dependencies and peer code review verifies new datastore client implementations or configuration changes.
</enforcement>