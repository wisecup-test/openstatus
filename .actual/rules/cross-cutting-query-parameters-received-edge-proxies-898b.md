---
version: 2
name: cross-cutting-query-parameters-received-edge-proxies-898b
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Query Parameters Received Edge Proxies Dynamic"
metadata:
  actual:
    adr_id: 898bace8-36ad-4de3-b630-7f75debb168a
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Query Parameters Received Edge Proxies Dynamic

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Query parameters received by edge proxies or dynamic routes MUST undergo validation against schemas compatible with the shared database definitions before executing database operations.

### Verify

```bash
# Discover and run the workspace type-checking script to ensure all references to @openstatus/db and its schema exports compile cleanly.
# Discover and run the project static analysis linting script to validate module import boundaries and ensure no unauthorized persistence libraries are imported.
```

**Accept when:**
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>