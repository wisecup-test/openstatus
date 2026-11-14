---
version: 2
name: cross-cutting-application-route-handlers-domain-utilit-1e4f
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Application Route Handlers Domain Utility Modules"
metadata:
  actual:
    adr_id: 1e4f5367-8eb2-4381-a408-0490aba913a8
    status: accepted
    candidate_group_id: 6a59cfe8-1e88-4c01-871a-1db91c9d4ce9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Application Route Handlers Domain Utility Modules

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application route handlers, scheduled tasks, and domain utility modules that query relational storage or reference database entity types.

### Rules

- **R-DB-001** MUST_NOT: Application route handlers and domain utility modules MUST NOT instantiate direct, unmanaged database client connections outside the centralized client exported by @openstatus/db.

### Verify

```bash
# Discover and execute the project static analysis and type verification scripts
# Discover and run the repository test suite
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated CI pipelines, peer code reviews, and automated blocking of pull requests introducing localized database clients or unmanaged connections.
</enforcement>