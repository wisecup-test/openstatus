---
version: 2
name: cross-cutting-high-frequency-event-tables-queried-4657
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: High Frequency Event Tables Queried Temporal"
metadata:
  actual:
    adr_id: 4657ee52-36a2-4ade-92bd-b9c9c0c326c7
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: High Frequency Event Tables Queried Temporal

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier, including relational entities representing multi-tenant resources, event telemetry, external service components, and status reports (excluding in-memory ephemeral cache stores, stateless client-side data structures, and transit payload models).

### Rules

- **R-DRIZZLE-001** SHOULD: High-frequency event tables queried by temporal ranges declare composite indices that combine the tenant partition key and the event timestamp column.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>