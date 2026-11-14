---
version: 2
name: cross-cutting-schema-validation-definitions-refine-str-208f
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Schema Validation Definitions Refine String Fields"
metadata:
  actual:
    adr_id: 208fd4ac-15e0-4dc0-be44-19b1aa3d25ad
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Schema Validation Definitions Refine String Fields

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for relational table schema definitions, database indexing, and domain model validation across persistence packages.

### Rules

- **R-DRZ-001** SHOULD: Schema validation definitions refine string fields using z.string() and associated constraints to ensure payload sanitization at domain entry points.

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