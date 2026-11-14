---
version: 2
name: cross-cutting-route-handlers-not-bypass-openstatus-abbc
description: "Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Route Handlers Not Bypass Openstatus Establish"
metadata:
  actual:
    adr_id: abbc0693-bd88-4f05-906a-9d595b264de4
    status: accepted
    candidate_group_id: 87c03f00-1745-43fe-a744-947f46b1adf4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/v1/**
---

# Adoption of @openstatus/db for Centralized Database Schema and Transactional Persistence: Route Handlers Not Bypass Openstatus Establish

Scope: applies to `apps/server/src/routes/v1/**`.

These rules are ALWAYS ACTIVE for route handlers, server services, and backend modules interacting with persistent data models and requiring database queries, schema validation, and atomic state mutations.

### Rules

- **R-DB-001** MUST_NOT: Route handlers MUST NOT bypass @openstatus/db to establish independent database connections or query drivers.

### Verify

```bash
# Discover and run the repository type-checking script to ensure all @openstatus/db schema references and transaction query calls compile cleanly.
# Discover and execute the repository test runner targeting route integration suites to verify atomic database transactions and schema interactions.
# Discover and run the project linter to ensure no unapproved direct database imports or raw driver calls bypass @openstatus/db.
```

**Accept when:**
- Type checking completes with zero diagnostic errors across all route handlers importing @openstatus/db.
- Integration tests pass verifying that multi-entity write operations commit atomically and roll back completely upon failure.
- Static analysis confirms all database operations route through @openstatus/db without unauthorized direct database connections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>