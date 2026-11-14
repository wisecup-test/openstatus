---
version: 2
name: cross-cutting-before-implementing-updating-data-access-b712
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Before Implementing Updating Data Access Components"
metadata:
  actual:
    adr_id: b712c6a2-350a-46e2-ba8c-9004c3908ec0
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Before Implementing Updating Data Access Components

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, authentication integrations, proxies, and operational scripts requiring relational persistence or schema definitions.

### Rules

- **R-PERSIST-001** MUST: Before implementing or updating data access components, inspect the repository lock artifact to determine the exact resolved versions of persistence packages and verify API compatibility against official documentation.
- **R-PERSIST-002** MUST: Import schema models and database client instances strictly from designated subpath exports of the internal database package.
- **R-PERSIST-003** MUST: Ensure serverless and edge runtime execution boundaries utilize appropriate connection pooling and adapter configurations exposed by the database module.

### Verify

```bash
# Discover and execute static analysis and type-checking suites across all workspace packages
# Run automated integration test suites to validate persistence queries and schema integrity
# Execute dependency boundary inspection scripts to confirm no unapproved database drivers are imported
```

**Accept when:**
- Static analysis confirms that database queries, transactions, and relational schema imports originate strictly from the @openstatus/db package.
- Automated integration test suites pass without database connection pool exhaustion or schema mismatch errors.
- Repository dependency boundary validation verifies no direct database driver dependencies exist in consuming application modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>