---
version: 2
name: cross-cutting-engineering-team-discover-repository-dep-0c69
description: "Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Engineering Team Discover Repository Dependency Lock"
metadata:
  actual:
    adr_id: 0c69c206-55af-496d-89ca-9d4e3c9ecbc4
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

# Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Engineering Team Discover Repository Dependency Lock

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DPD-001** MUST: The engineering team MUST discover the repository dependency lock artifact and verify the exact resolved version of all database access dependencies prior to implementation.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>