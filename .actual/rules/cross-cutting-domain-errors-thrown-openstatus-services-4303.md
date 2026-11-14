---
version: 2
name: cross-cutting-domain-errors-thrown-openstatus-services-4303
description: "Primary Datastore Access Delegation via @openstatus/services: Domain Errors Thrown Openstatus Services Invocations"
metadata:
  actual:
    adr_id: 4303f217-74c0-4754-80c4-7653be541f96
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Domain Errors Thrown Openstatus Services Invocations

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures performing read or write operations against primary datastores.

### Rules

- **R-OPEN-001** MUST: Domain errors thrown by @openstatus/services invocations inside route procedures MUST be captured and translated into standard procedure errors using toTRPCError or throwFromException.

### Verify

```bash
# Discover the repository build and type-checking scripts from the project configuration and execute them
npm run build || pnpm build || yarn build
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and run the test suite scripts declared in the workspace configuration
npm test || pnpm test || yarn test
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>