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

These rules are ALWAYS ACTIVE for all server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-PLAN-001** MUST_NOT: Handlers MUST NOT define local, hardcoded tier ceilings or duplicate quota thresholds outside of the canonical plan schema module.

### Verify

```bash
# Discover and run workspace type check / static analysis script
# Discover and run project test runner across all limit handler test suites
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and architectural code reviews.
</enforcement>