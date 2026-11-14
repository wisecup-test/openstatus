---
version: 2
name: cross-cutting-handlers-querying-primary-datastore-exec-18df
description: "Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Handlers Querying Primary Datastore Execute Parameterized"
metadata:
  actual:
    adr_id: 18df9b07-3125-410b-bce1-9646ea1c5af9
    status: accepted
    candidate_group_id: 01844da4-922e-415b-8a8a-f2dcfc9a0024
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
---

# Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Handlers Querying Primary Datastore Execute Parameterized

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Handlers querying the primary datastore MUST execute parameterized queries through database client Select and Get methods, passing authentication tokens and entity identifiers as bound parameters.

### Verify

```bash
# Discover and run the repository test suite covering server request handlers and database integration.
# Discover and run the project static analysis and linting checks to ensure compliance with database query conventions.
# Discover and execute database migration verification scripts against the local development datastore.
```

**Accept when:**
- All probe handler integration tests pass against primary datastore queries with valid and invalid tokens.
- Ingest context retrieval binds database records to typed domain structures and returns unauthenticated errors on invalid tokens.
- Static analysis checks complete with zero policy violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>