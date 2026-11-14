---
version: 2
name: cross-cutting-when-adopting-interacting-versioned-depe-4b10
description: "Drizzle ORM and LibSQL Database Access Layer: When Adopting Interacting Versioned Dependencies Object"
metadata:
  actual:
    adr_id: 4b1092b6-3eda-47c2-a627-eddfbc66d247
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: When Adopting Interacting Versioned Dependencies Object

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for relational database access, querying, migrations, local replica synchronization, and data seeding in database packages.

### Rules

- **R-DRIZZLE-001** MUST: When adopting or interacting with versioned dependencies for the object-relational mapping and database driver layers, the consumer MUST inspect the repository lock artifact to determine the exact resolved version and verify supported APIs prior to implementation.

### Verify

```bash
# Discover and execute the repository type-checking script to validate database types
# Discover and run the project's test suite to verify database connection and operations
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>