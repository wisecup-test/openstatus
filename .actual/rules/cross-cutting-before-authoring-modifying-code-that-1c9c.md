---
version: 2
name: cross-cutting-before-authoring-modifying-code-that-1c9c
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Before Authoring Modifying Code That Consumes"
metadata:
  actual:
    adr_id: 1c9cdd12-a336-4d66-b6c7-c0d373614092
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Before Authoring Modifying Code That Consumes

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Before authoring or modifying code that consumes shared dependencies, the consumer MUST inspect the workspace dependency manifest and repository resolution artifact to determine the exact locked dependency versions.
- **R-OPENSTATUS-002** MUST: All application route handlers, middleware proxies, authentication providers, and metadata generators requiring database access or schema definitions MUST utilize the shared @openstatus/db workspace module.

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