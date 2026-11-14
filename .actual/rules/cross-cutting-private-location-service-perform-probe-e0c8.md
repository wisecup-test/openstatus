---
version: 2
name: cross-cutting-private-location-service-perform-probe-e0c8
description: "Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Private Location Service Perform Probe Authentication"
metadata:
  actual:
    adr_id: e0c82cae-0863-4cfb-8a78-31d003e14db6
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

# Direct Primary Datastore Access via internal/database for Probe Ingestion and Authentication: Private Location Service Perform Probe Authentication

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DPD-001** MUST: The private location service MUST perform probe authentication and ingest metadata retrieval directly against the primary datastore using the internal/database module rather than introducing an intermediate caching tier.

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