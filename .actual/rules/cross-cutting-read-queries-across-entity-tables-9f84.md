---
version: 2
name: cross-cutting-read-queries-across-entity-tables-9f84
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Read Queries Across Entity Tables Utilize"
metadata:
  actual:
    adr_id: 9f84e694-08c5-46a4-b6a3-0fe3aa64aebf
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Read Queries Across Entity Tables Utilize

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for all route handlers and server services requiring database queries, schema validation, and atomic state mutations, as well as backend modules interacting with persistent data models.

### Rules

- **R-OPENSTATUS-001** MUST: Read queries across entity tables MUST utilize fluent query builder methods provided by the @openstatus/db client rather than raw database driver queries.

### Verify

```bash
# Discover and run the repository type-checking script
# Discover and execute the repository test runner targeting route integration suites
# Discover and run the project linter to ensure no unapproved direct database imports or raw driver calls bypass @openstatus/db
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all route handlers importing @openstatus/db.
- Integration tests pass verifying that multi-entity write operations commit atomically and roll back completely upon failure.
- Static analysis confirms all database operations route through @openstatus/db without unauthorized direct database connections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>