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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DZ-001** MUST: Discover the ecosystem lock file and verify the resolved version of drizzle-zod and peer dependencies before implementation.

### Verify

```bash
# 1. Find dependency manifest and lock file in repository
# 2. Inspect lock/resolution artifact for exact version of drizzle-zod and peer dependencies
# 3. Verify types and test suite
npm test || yarn test || pnpm test
npm run typecheck || yarn typecheck || pnpm typecheck
```

**Accept when:**
- Validation schemas generated from database table definitions successfully validate valid entity records.
- Malformed input payloads are rejected with structured parse errors before reaching database execution operations.
- Static type checking verifies that derived validation types match corresponding database insert and select definitions without type errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database schemas and ingestion boundaries must adhere to lock-version grounding and derived validation rules.
</enforcement>