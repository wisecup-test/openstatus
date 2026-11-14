---
version: 2
name: cross-cutting-execute-input-validation-schema-parsing-f56d
description: "Adoption of drizzle-zod for Database Schema Validation: Execute Input Validation Schema Parsing Methods"
metadata:
  actual:
    adr_id: f56d1ac7-801c-49cd-a8c1-3a0d3acbc623
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Execute Input Validation Schema Parsing Methods

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, data validation modules defining entity constraints, and ingestion boundaries handling API payloads destined for database persistence.

### Rules

- **R-DZ-001** MUST: Execute input validation using schema parsing methods at application ingestion boundaries before executing database persistence operations.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites, peer code review, and static type analysis.
</enforcement>