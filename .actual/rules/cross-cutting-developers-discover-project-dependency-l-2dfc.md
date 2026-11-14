---
version: 2
name: cross-cutting-developers-discover-project-dependency-l-2dfc
description: "drizzle-orm with LibSQL for Database Operations: Developers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 2dfc74d3-288c-48b8-a8b5-6fb1523b1d88
    status: accepted
    candidate_group_id: 546e7b81-0328-4a21-88db-87a6b81e682c
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# drizzle-orm with LibSQL for Database Operations: Developers Discover Project Dependency Lock Artifact

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Developers MUST discover the project dependency lock artifact and resolve the exact installed versions of drizzle-orm, drizzle-orm/libsql, and @libsql/client before introducing or modifying database interaction code.

### Verify

```bash
# Discover and run the project build, type check, linter, and test suites using the project build tool discovered from the repository manifest.
```

**Accept when:**
- All database queries, seed operations, and client connections compile with zero type errors against the shared schema.
- Database synchronization clients successfully initialize with configured local storage parameters and remote synchronization endpoints.
- Automated tests covering database interactions and workspace schema validations pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration pipeline executes type checks and test suites, and peer code review verifies adherence to database client instantiation standards and schema binding rules.
</enforcement>