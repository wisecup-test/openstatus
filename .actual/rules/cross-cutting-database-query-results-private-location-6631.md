---
version: 2
name: cross-cutting-database-query-results-private-location-6631
description: "Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Database Query Results Private Location Regions"
metadata:
  actual:
    adr_id: 6631dd0e-abae-4384-8e2c-3ffa4dd20d40
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

# Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Database Query Results Private Location Regions

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Database query results for private location regions and monitor entities MUST be bound directly to the typed domain structures defined in the internal/database module.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration test suites and peer code reviews.
</enforcement>