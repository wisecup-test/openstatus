---
version: 2
name: cross-cutting-multi-table-write-operations-interdepend-72c1
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Multi Table Write Operations Interdependent Record"
metadata:
  actual:
    adr_id: 72c10fa4-2f9e-44aa-8e79-1301223b975e
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Multi Table Write Operations Interdependent Record

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for all route handlers, server services, and backend modules interacting with persistent data models requiring database queries, schema validation, and atomic state mutations.

### Rules

- **R-DB-001** MUST: Multi-table write operations and interdependent record modifications MUST execute within an atomic transaction context using the transaction handle provided by @openstatus/db.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline executes repository type checks and integration test suites. Peer code review verifies adherence to transactional patterns and direct consumption of @openstatus/db/src/schema.
</enforcement>