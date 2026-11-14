---
version: 2
name: cross-cutting-extend-generated-schemas-custom-field-ab86
description: "Adoption of drizzle-zod for Database Schema Validation: Extend Generated Schemas Custom Field Constraints"
metadata:
  actual:
    adr_id: ab865958-da1b-4202-906a-3b3bd5b017f0
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Extend Generated Schemas Custom Field Constraints

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, data validation modules, and ingestion boundaries handling API payloads destined for database persistence.

### Rules

- **R-DRIZZLE-ZOD-001** SHOULD: Extend generated schemas with custom field constraints, default values, and transformations when payload specifications diverge from raw table types.
- **R-DRIZZLE-ZOD-002** MANDATORY: Ground all dependency updates against repository lock artifacts and verify peer dependency ranges during updates.
- **R-DRIZZLE-ZOD-003** MANDATORY: Derive baseline schemas using library generator functions and apply refinement methods to enforce business constraints on specific fields.
- **R-DRIZZLE-ZOD-004** MANDATORY: Group table definitions and their associated validation schemas within the same domain directory to preserve structural co-location.
- **R-DRIZZLE-ZOD-005** MANDATORY: Explicitly pick, omit, or extend generated schema fields rather than directly exposing raw generated table schemas to public routes.

### Verify

```bash
# Discover and run the project test suite to verify that validation schemas correctly parse valid payloads and reject malformed inputs.
# Discover and execute the type-checking and schema validation scripts to confirm that derived schemas conform to database table types.
if [ -f package.json ]; then
  npm test || npx jest || pnpm test
  npm run typecheck || npx tsc --noEmit || pnpm typecheck
fi
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory for all changes involving database schema validation and payload ingestion.
</enforcement>