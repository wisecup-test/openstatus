---
version: 2
name: cross-cutting-runtime-input-validation-schemas-databas-0eea
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Runtime Input Validation Schemas Database Entities"
metadata:
  actual:
    adr_id: 0eeab7c7-cd3e-416f-af37-ecec452098b6
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Runtime Input Validation Schemas Database Entities

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Runtime input validation schemas for database entities MUST be derived directly from table schemas using drizzle-zod and zod to prevent contract divergence between persistence and validation boundaries.
- **R-DRIZZLE-002** MANDATORY: Consumers MUST execute the lock-version grounding process before writing code that uses a versioned library: (1) Find the dependency manifest, (2) Identify the build tool, (3) Inspect the repository lock or resolution artifact for exact resolved versions, (4) Look up official documentation for that exact version, (5) Confirm API existence before calling, and (6) Re-run per dependency at point of use for version-sensitive behavior.
- **R-DRIZZLE-003** SHOULD: Ensure composite index declarations reflect filtering and sorting access paths across relational foreign keys and timestamp attributes.
- **R-DRIZZLE-004** SHOULD: Export derived validation schemas from dedicated schema validation modules to provide clean ingestion boundaries for consuming applications.

### Verify

```bash
# Discover and execute the project database test suite to verify schema definitions and indexing
# Discover and execute the repository type validation and linter checks to ensure schema and validation contract compliance
```

**Accept when:**
- Database table schemas compile without type errors and composite indexes match declared access patterns.
- Derived validation schemas correctly validate compliant payloads and reject malformed input matching table constraints.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline and peer code review.
</enforcement>