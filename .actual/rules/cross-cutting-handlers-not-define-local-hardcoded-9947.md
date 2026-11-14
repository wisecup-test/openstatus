---
version: 2
name: cross-cutting-handlers-not-define-local-hardcoded-9947
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Handlers Not Define Local Hardcoded Tier"
metadata:
  actual:
    adr_id: 9947cb2a-a2bf-4a7f-bd2b-18d2ad371559
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Handlers Not Define Local Hardcoded Tier

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-RPC-001** MUST_NOT: Handlers MUST NOT define local, hardcoded tier ceilings or duplicate quota thresholds outside of the canonical plan schema module.
- **R-RPC-002** MUST: Consume all plan limit calculations and entitlement thresholds directly from the canonical plan schema exports rather than derived through bespoke database queries.
- **R-RPC-003** MUST: Structure quota evaluation logic into dedicated limit handler modules within each RPC feature domain.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# to verify that RPC limit handlers import quota definitions solely from the canonical plan schema module.
npm run typecheck

# Discover and run the project test runner across all limit handler test suites
# to verify quota enforcement logic against defined plan schemas.
npm test
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests containing decentralized quota constants or hardcoded limit definitions must be rejected.
</enforcement>