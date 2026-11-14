---
version: 2
name: cross-cutting-procedures-that-consume-list-operations-80af
description: "Primary Datastore Access Delegation via @openstatus/services: Procedures That Consume List Operations Openstatus"
metadata:
  actual:
    adr_id: 80afdd1e-d1aa-48f5-99ad-95be123f873d
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Procedures That Consume List Operations Openstatus

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures performing read or write operations against primary datastores.

### Rules

- **R-OPENSTATUS-001** MUST: API procedures that consume list operations from @openstatus/services MUST explicitly provide pagination constraints or bounded sentinel limits to prevent unbounded datastore scans.

### Verify

```bash
# Discover the repository build and type-checking scripts from the project configuration and execute them
npm run build || pnpm build || yarn build
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and run test suite scripts
npm test || pnpm test || yarn test
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification. All API procedures must delegate primary datastore list operations through @openstatus/services with explicit pagination constraints or bounded sentinel limits.
</enforcement>