---
version: 2
name: cross-cutting-database-interaction-code-not-bypass-2230
description: "drizzle-orm with LibSQL for Database Operations: Database Interaction Code Not Bypass Drizzle"
metadata:
  actual:
    adr_id: 22304c9c-e21d-467e-a02b-e74fcad1f337
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Database Interaction Code Not Bypass Drizzle

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Database interaction code MUST_NOT bypass the drizzle-orm client abstraction to execute untyped or raw SQL queries directly against underlying connection sockets unless explicitly documented as a performance-critical exception.

### Verify

```bash
# Discover and run the project test suite using the project build tool discovered from the repository manifest.
# Discover and run the project type checking script to verify schema and query type safety across all database modules.
# Discover and run the project linter and validation checks to ensure compliance with database import and module boundary rules.
```

**Accept when:**
- All database queries, seed operations, and client connections compile with zero type errors against the shared schema.
- Database synchronization clients successfully initialize with configured local storage parameters and remote synchronization endpoints.
- Automated tests covering database interactions and workspace schema validations pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipeline executing type checks and test suites, and peer code review.
</enforcement>