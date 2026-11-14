---
version: 2
name: cross-cutting-discover-ecosystem-lock-file-verify-0c36
description: "Adoption of drizzle-zod for Database Schema Validation: Discover Ecosystem Lock File Verify Resolved"
metadata:
  actual:
    adr_id: 0c367096-86fc-4ab4-a55b-73d3c1a39103
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Discover Ecosystem Lock File Verify Resolved

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, data validation modules, and ingestion boundaries handling API payloads destined for database persistence.

### Rules

- **R-DZ-001** MUST: Discover the ecosystem lock file and verify the resolved version of drizzle-zod and peer dependencies before implementation.
- **R-DZ-002** MUST: Derive baseline schemas using library generator functions and apply refinement methods to enforce business constraints on specific fields.
- **R-DZ-003** MUST: Group table definitions and their associated validation schemas within the same domain directory to preserve structural co-location.

### Verify

```bash
# Discover and run the project test suite to verify validation schemas
# Run type-checking and schema validation scripts to confirm derived schemas conform to database table types
npm test
npm run typecheck
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests lacking derived validation schemas for newly introduced database tables are blocked from merging.
</enforcement>