---
version: 2
name: cross-cutting-developers-discover-repository-dependenc-cf83
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Developers Discover Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: cf83d60b-9819-4eff-b9cf-5651dc98edb3
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Developers Discover Repository Dependency Manifest Lock

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for route handlers, server services, and backend modules interacting with persistent data models requiring database queries, schema validation, and atomic state mutations.

### Rules

- **R-DB-001** MUST: Developers MUST discover the repository dependency manifest and lock artifact to verify the exact resolved version of @openstatus/db and its underlying database client before introducing or modifying data access routines.

### Verify

```bash
# Discover and run the repository type-checking script
# Discover and execute the repository test runner targeting route integration suites
# Discover and run the project linter to ensure no unapproved direct database imports
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all route handlers importing @openstatus/db.
- Integration tests pass verifying that multi-entity write operations commit atomically and roll back completely upon failure.
- Static analysis confirms all database operations route through @openstatus/db without unauthorized direct database connections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>