---
version: 2
name: cross-cutting-application-route-handlers-proxy-layers-70f9
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Application Route Handlers Proxy Layers Utility"
metadata:
  actual:
    adr_id: 70f9da6e-14f5-481e-841b-18c2578aa304
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Application Route Handlers Proxy Layers Utility

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, middleware proxies, authentication providers, and metadata generators requiring database access or schema definitions.

### Rules

- **R-DB-001** MUST_NOT: Application route handlers, proxy layers, and utility modules MUST NOT instantiate separate database connection pools or import raw database drivers directly.

### Verify

```bash
# Discover and run the workspace type-checking script to ensure all references to @openstatus/db and its schema exports compile cleanly.
# Discover and run the project static analysis linting script to validate module import boundaries and ensure no unauthorized persistence libraries are imported.
```

**Accept when:**
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace type checking and static analysis import boundary rules must pass without violation.
</enforcement>