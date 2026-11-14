---
version: 2
name: cross-cutting-protected-procedures-calling-openstatus-ead7
description: "Primary Datastore Access Delegation via @openstatus/services: Protected Procedures Calling Openstatus Services Adapt"
metadata:
  actual:
    adr_id: ead74dde-69f4-49ab-85d8-2620c9c868dd
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Protected Procedures Calling Openstatus Services Adapt

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures performing read or write operations against primary datastores.

### Rules

- **R-PRC-001** MUST: Protected procedures calling @openstatus/services MUST adapt the incoming request context to the service boundary context using the toServiceCtx adapter function.

### Verify

```bash
# Discover and run the project build and type-checking scripts
npm run build
npm run test
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checks and static lint rules verify service interface compliance.
</enforcement>