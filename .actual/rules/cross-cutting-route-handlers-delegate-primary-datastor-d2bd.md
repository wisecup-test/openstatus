---
version: 2
name: cross-cutting-route-handlers-delegate-primary-datastor-d2bd
description: "Primary Datastore Access Delegation via @openstatus/services: Route Handlers Delegate Primary Datastore Queries"
metadata:
  actual:
    adr_id: d2bda6f9-7c3d-4f2c-bab3-3653c2de20c1
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Route Handlers Delegate Primary Datastore Queries

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures performing read or write operations against primary datastores.

### Rules

- **R-DS-001** MUST: API route handlers MUST delegate all primary datastore queries and persistence operations to @openstatus/services domain modules rather than executing direct database queries or accessing low-level data clients.

### Verify

```bash
# Discover and execute project build and type-checking scripts
pnpm build
pnpm typecheck

# Discover and run the workspace test suite
pnpm test
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>