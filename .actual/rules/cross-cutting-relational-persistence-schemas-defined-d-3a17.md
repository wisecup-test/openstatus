---
version: 2
name: cross-cutting-relational-persistence-schemas-defined-d-3a17
description: "Drizzle ORM SQLite Schema Modeling: Relational Persistence Schemas Defined Drizzle Orm"
metadata:
  actual:
    adr_id: 3a17e93d-7fc3-49aa-9597-71a316b5aedb
    status: accepted
    candidate_group_id: a9a191ea-ecd5-4f87-857c-a1a6208051f5
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/viewers/**
---

# Drizzle ORM SQLite Schema Modeling: Relational Persistence Schemas Defined Drizzle Orm

Scope: applies to `packages/db/src/schema/viewers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Relational persistence schemas MUST be defined using Drizzle ORM SQLite core table definitions to establish compile-time type safety across database entities.

### Verify

```bash
# Discover and run the project type checking script to verify schema type correctness across all consuming packages.
# Discover and run the database schema verification or migration generation command to validate table constraints.
# Discover and run the automated test suite to confirm session cascading and timestamp deserialization.
```

**Accept when:**
- Static type analysis passes with zero errors across all database schema definitions and downstream consumers.
- Database schema migration checks confirm valid SQLite DDL generation with foreign key references intact.
- Integration tests succeed confirming cascade deletion of session records upon user deletion.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>