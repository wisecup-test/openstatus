---
version: 2
name: cross-cutting-before-implementing-updating-features-de-3a0a
description: "Primary Datastore Access Delegation via @openstatus/services: Before Implementing Updating Features Dependent External"
metadata:
  actual:
    adr_id: 3a0af514-81ca-4789-a2f6-f990673418b4
    status: accepted
    candidate_group_id: ba6c3eef-91c9-4cd0-a5dc-ed506fd87311
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Primary Datastore Access Delegation via @openstatus/services: Before Implementing Updating Features Dependent External

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedures performing read or write operations against primary datastores.

### Rules

- **R-DS-001** MUST: Before implementing or updating features dependent on external or workspace packages, developers MUST discover the dependency manifest and authoritative repository lock artifact to verify the exact resolved version.

### Verify

```bash
# Discover and execute repository build and type-checking scripts
# Example based on workspace config:
pnpm typecheck
pnpm test
```

**Accept when:**
- All API procedures interacting with primary datastores delegate persistence operations through @openstatus/services domain modules without direct datastore client invocation.
- Static analysis and type checking confirm zero type errors across context adapters, input validators, and service invocations.
- Automated test suites pass with verification of service error handling and tenant boundary isolation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>