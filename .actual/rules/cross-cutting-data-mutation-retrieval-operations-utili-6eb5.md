---
version: 2
name: cross-cutting-data-mutation-retrieval-operations-utili-6eb5
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Data Mutation Retrieval Operations Utilize Typed"
metadata:
  actual:
    adr_id: 6eb5fa33-e087-48eb-bfb0-66eab98e301f
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Data Mutation Retrieval Operations Utilize Typed

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, middleware proxies, authentication providers, and metadata generators requiring database access or schema definitions, as well as workspace packages that participate in shared relational data access.

### Rules

- **R-DB-001** SHOULD: Data mutation and retrieval operations SHOULD utilize the typed query builder interfaces exposed by the shared database module rather than unstructured string queries.

### Verify

```bash
# Discover and run the workspace type-checking script to ensure all references to @openstatus/db and its schema exports compile cleanly.
npm run typecheck

# Discover and run the project static analysis linting script to validate module import boundaries and ensure no unauthorized persistence libraries are imported.
npm run lint
```

**Accept when:**
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

<enforcement>
Claude Code MUST NOT skip or defer verification. All relational data interactions and schema references within the application must utilize the shared @openstatus/db workspace module, and automated type-checking and linting checks must pass.
</enforcement>