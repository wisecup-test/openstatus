---
version: 2
name: cross-cutting-developers-not-author-standalone-duplica-267f
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Developers Not Author Standalone Duplicate Validation"
metadata:
  actual:
    adr_id: 267f9ed3-e671-433c-9555-216b4eae29ee
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Developers Not Author Standalone Duplicate Validation

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for relational table schema definitions, database indexing, and domain model validation across persistence packages.

### Rules

- **R-DRIZZLE-001** MUST_NOT: Developers MUST NOT author standalone duplicate validation schemas that recreate database table fields manually without deriving from the underlying table definitions.

### Verify

```bash
# Discover and execute the project database test suite to verify schema definitions and indexing
# Discover and execute the repository type validation and linter checks to ensure schema and validation contract compliance
```

**Accept when:**
- Database table schemas compile without type errors and composite indexes match declared access patterns.
- Derived validation schemas correctly validate compliant payloads and reject malformed input matching table constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>