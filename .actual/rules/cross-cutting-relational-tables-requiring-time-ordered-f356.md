---
version: 2
name: cross-cutting-relational-tables-requiring-time-ordered-f356
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Relational Tables Requiring Time Ordered Filtering"
metadata:
  actual:
    adr_id: f3568c36-57e0-4169-afa2-5f2570c4b355
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Relational Tables Requiring Time Ordered Filtering

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for relational table schema definitions, database indexing, and domain model validation across persistence packages.

### Rules

- **R-DRIZZLE-001** MUST: Relational tables requiring time-ordered filtering or multi-column lookups MUST declare explicit composite indexes using the index().on() schema definition API.

### Verify

```bash
# Discover and execute the project database test suite to verify schema definitions and indexing
# Discover and execute the repository type validation and linter checks to ensure schema and validation contract compliance
```

**Accept when:**
- Database table schemas compile without type errors and composite indexes match declared access patterns.
- Derived validation schemas correctly validate compliant payloads and reject malformed input matching table constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>