---
version: 2
name: cross-cutting-developers-inspect-repository-lock-resol-8e8d
description: "Drizzle ORM Adoption for Database Schema and Validation Architecture: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 8e8d059d-8890-42e9-a8f3-57822a22ea35
    status: accepted
    candidate_group_id: 3aa69e6a-bf58-4fe2-b072-4d30df473306
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/external_services/**
---

# Drizzle ORM Adoption for Database Schema and Validation Architecture: Developers Inspect Repository Lock Resolution Artifact

Scope: applies to `packages/db/src/schema/external_services/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Developers MUST inspect the repository lock or resolution artifact to determine the exact resolved versions of drizzle-orm, drizzle-zod, and zod prior to implementing schema or validation changes.

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