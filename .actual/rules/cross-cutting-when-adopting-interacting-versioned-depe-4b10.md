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
- **R-DRIZZLE-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-DRIZZLE-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo declaring ranges; (2) Identify the build tool from the manifest; (3) Inspect the repository lock or resolution artifact to determine the exact resolved version; (4) Look up official documentation, changelog, or public API reference for that exact version without relying on training-data recall; (5) Confirm every API, class, or function called exists in that exact version's documentation before use; (6) For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-DRIZZLE-004** MUST: Define all relational entities and schema tables in centralized schema modules, deriving TypeScript types directly from schema models.
- **R-DRIZZLE-005** MUST: Initialize database clients through centralized configuration modules that inject validated environment variables for connection URLs, authentication tokens, and replication intervals.

### Verify

```bash
# Discover and execute the repository type-checking script to validate that all database queries, migrations, and schema definitions conform to Drizzle ORM and LibSQL client types.
# Discover and run the project's test suite to verify database connection initialization, schema migration execution, and query operations.
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database access must utilize Drizzle ORM constructs and designated LibSQL client abstractions, and pull requests introducing raw unmapped database queries or unapproved driver dependencies are blocked.
</enforcement>