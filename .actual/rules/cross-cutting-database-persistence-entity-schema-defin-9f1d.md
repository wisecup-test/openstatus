---
version: 2
name: cross-cutting-database-persistence-entity-schema-defin-9f1d
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Database Persistence Entity Schema Definitions Transactional"
metadata:
  actual:
    adr_id: 9f1d849b-3eba-4e6e-a02d-31f0ffadf33f
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Database Persistence Entity Schema Definitions Transactional

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for all route handlers, server services, and backend modules requiring database queries, schema validation, and atomic state mutations.

### Rules

- **R-DB-001** MUST: All database persistence, entity schema definitions, and transactional mutations across server route handlers MUST be implemented using the shared @openstatus/db module and @openstatus/db/src/schema.

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
Claude Code MUST NOT skip or defer verification. All database persistence and transactional mutations must strictly adhere to the shared @openstatus/db module.
</enforcement>