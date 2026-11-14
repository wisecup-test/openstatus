---
version: 2
name: cross-cutting-authentication-adapters-requiring-databa-5157
description: "Adoption of @openstatus/db as Centralized Persistence and Schema Module: Authentication Adapters Requiring Database Connectivity Consume"
metadata:
  actual:
    adr_id: 5157365c-475c-41b4-b9be-51cb3fab7aae
    status: accepted
    candidate_group_id: da7d71f2-7d1f-437e-9659-519d2966bdda
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of @openstatus/db as Centralized Persistence and Schema Module: Authentication Adapters Requiring Database Connectivity Consume

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all authentication adapters and application components requiring database connectivity.

### Rules

- **R-DB-001** MUST: Authentication adapters requiring database connectivity MUST consume the centralized database client instance provided by @openstatus/db.

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