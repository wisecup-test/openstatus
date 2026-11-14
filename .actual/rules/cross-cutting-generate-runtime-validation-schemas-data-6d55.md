---
version: 2
name: cross-cutting-generate-runtime-validation-schemas-data-6d55
description: "Adoption of drizzle-zod for Database Schema Validation: Generate Runtime Validation Schemas Database Entities"
metadata:
  actual:
    adr_id: 6d55652d-c502-4409-afda-01701ef1bbe1
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Generate Runtime Validation Schemas Database Entities

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, data validation modules, and ingestion boundaries handling API payloads destined for database persistence.

### Rules

- **R-DZ-001** MUST: Generate runtime validation schemas for database entities directly from table definitions using drizzle-zod.

### Verify

```bash
# Discover and run the project test suite to verify validation schemas
# Discover and execute type-checking and schema validation scripts
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests lacking derived validation schemas for newly introduced database tables are blocked from merging.
</enforcement>