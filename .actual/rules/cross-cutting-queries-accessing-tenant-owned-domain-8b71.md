---
version: 2
name: cross-cutting-queries-accessing-tenant-owned-domain-8b71
description: "Centralized Relational Data Access with @openstatus/db: Queries Accessing Tenant Owned Domain Entities"
metadata:
  actual:
    adr_id: 8b71be17-c500-4f6a-952a-c3412b41ab74
    status: accepted
    candidate_group_id: 0e1a37eb-0f9b-4ab0-9e8c-680ac9ec4729
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/auth/**
      - src/router/**
---

# Centralized Relational Data Access with @openstatus/db: Queries Accessing Tenant Owned Domain Entities

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, authentication helpers, and data access logic executing queries or mutations against domain entities requiring multi-tenant scoping.

### Rules

- **R-DB-001** MUST: Queries accessing tenant-owned domain entities MUST enforce workspace isolation by constraining where conditions with the active workspace identifier provided by procedure context.

### Verify

```bash
# Discover and run static type analysis and test validation commands from the repository manifest
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification. All queries accessing tenant-owned domain entities must strictly enforce workspace isolation via the procedure context.
</enforcement>