---
version: 2
name: cross-cutting-use-schema-generation-functions-produce-ddb8
description: "Adoption of drizzle-zod for Database Schema Validation: Use Schema Generation Functions Produce Insert"
metadata:
  actual:
    adr_id: ddb87bce-73e1-4e1c-afca-3cc1f95bd0b9
    status: accepted
    candidate_group_id: 863e57d5-9ef1-4bf1-b1e7-25ab8510ecfc
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-zod for Database Schema Validation: Use Schema Generation Functions Produce Insert

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DZ-001** MUST: Use schema generation functions to produce insert and select validation schemas corresponding to entity table definitions.

### Verify

```bash
# Discover and run the project test suite and type-checking scripts
npm test
npm run typecheck
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>