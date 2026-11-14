---
version: 2
name: cross-cutting-authentication-adapters-session-stores-i-d853
description: "Adoption of @openstatus/db Workspace Database and Schema Module: Authentication Adapters Session Stores Identity Verifiers"
metadata:
  actual:
    adr_id: d853556c-9583-44a4-aa40-3857ce1b9a4c
    status: accepted
    candidate_group_id: 8b59c3c7-1008-4fa2-beb3-471d2a2a9480
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of @openstatus/db Workspace Database and Schema Module: Authentication Adapters Session Stores Identity Verifiers

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application route handlers, middleware proxies, authentication providers, and metadata generators requiring database access or schema definitions, and workspace packages participating in shared relational data access.

### Rules

- **R-DB-001** MUST: Authentication adapters, session stores, and identity verifiers MUST bind directly to canonical entity schemas exported from the shared database module.

### Verify

```bash
# Discover and run the workspace type-checking script to ensure all references to @openstatus/db and its schema exports compile cleanly.
# Discover and run the project static analysis linting script to validate module import boundaries and ensure no unauthorized persistence libraries are imported.
```

**Accept when:**
- All application components query relational data exclusively through the @openstatus/db client instance and exported schema models.
- Static verification scripts pass without errors regarding module import boundaries or mismatched entity types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated workspace type checking and static analysis import boundary enforcement are mandatory.
</enforcement>