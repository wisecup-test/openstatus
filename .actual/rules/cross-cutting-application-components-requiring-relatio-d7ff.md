---
version: 2
name: cross-cutting-application-components-requiring-relatio-d7ff
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Application Components Requiring Relational Data Persistence"
metadata:
  actual:
    adr_id: d7ffbcfb-a4db-4f3f-9341-772121e52249
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Application Components Requiring Relational Data Persistence

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application components requiring relational data persistence, schema models, or query execution.

### Rules

- **R-DB-001** MUST: All application components requiring relational data persistence, schema models, or query execution MUST import exclusively from the shared workspace module @openstatus/db and its schema subpath.

### Verify

```bash
# Discover and run the workspace type-checking script
# Discover and run the project static analysis linting script
```

**Accept when:**
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated workspace type checking, static analysis import boundary rules, and peer code review.
</enforcement>