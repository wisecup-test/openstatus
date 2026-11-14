---
version: 2
name: cross-cutting-relational-persistence-operations-databa-8bf7
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Relational Persistence Operations Database Transactions Entity"
metadata:
  actual:
    adr_id: 8bf71441-a477-4add-bc3b-cf01c9297412
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Relational Persistence Operations Database Transactions Entity

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: All relational persistence operations, database transactions, and entity queries MUST be conducted exclusively through the centralized @openstatus/db package.

### Verify

```bash
# Discover and execute the repository static analysis and type-checking suites across all workspace packages
# Discover and run the project automated integration test suite
# Discover and execute the dependency boundary inspection script
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>