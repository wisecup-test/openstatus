---
version: 2
name: cross-cutting-database-table-definitions-relational-st-35ab
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Database Table Definitions Relational Structures Use"
metadata:
  actual:
    adr_id: 35ab6831-9aef-45c9-92c4-bc24a1e74c5d
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Database Table Definitions Relational Structures Use

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Database table definitions and relational structures MUST use drizzle-orm to establish schema architecture and define table properties.

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