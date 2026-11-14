---
version: 2
name: cross-cutting-database-entity-schema-definitions-impor-0751
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Database Entity Schema Definitions Imported Directly"
metadata:
  actual:
    adr_id: 07511150-e825-4c32-9f43-feefb0d1ba29
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Database Entity Schema Definitions Imported Directly

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Database entity schema definitions MUST be imported directly from @openstatus/db/src/schema rather than defined locally within route handlers.

### Verify

```bash
# Discover and run type checking
npm run typecheck
# Discover and run integration tests
npm test
# Discover and run linter
npm run lint
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all route handlers importing @openstatus/db.
- Integration tests pass verifying that multi-entity write operations commit atomically and roll back completely upon failure.
- Static analysis confirms all database operations route through @openstatus/db without unauthorized direct database connections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and peer code review enforce adherence to transactional patterns and direct consumption of @openstatus/db/src/schema.
</enforcement>