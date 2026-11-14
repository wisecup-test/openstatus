---
version: 2
name: cross-cutting-not-manually-recreate-standalone-validat-9f9e
description: "Adoption of drizzle-zod for Database Schema Validation: Not Manually Recreate Standalone Validation Schemas"
metadata:
  actual:
    adr_id: 9f9e1bdb-2d86-4b03-913d-8d734862f18f
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Not Manually Recreate Standalone Validation Schemas

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, data validation modules, and ingestion boundaries handling API payloads destined for database persistence.

### Rules

- **R-DRIZZLE-001** MUST_NOT: Manually recreate standalone validation schemas for persisted entity attributes when corresponding table definitions exist in the schema layer.

### Verify

```bash
# Discover and run the project test suite to verify validation schemas
# Run the type-checking and schema validation scripts to confirm derived schemas conform to database table types
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new database entities must include corresponding derived validation schemas.
</enforcement>