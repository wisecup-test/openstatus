---
version: 2
name: cross-cutting-any-newly-introduced-domain-entity-9e77
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Any Newly Introduced Domain Entity Requiring"
metadata:
  actual:
    adr_id: 9e77316e-1858-41c1-975a-2de68ad48b0a
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Any Newly Introduced Domain Entity Requiring

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for Server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-PLAN-001** MUST: Any newly introduced domain entity requiring quota constraints MUST define its entitlement boundaries within the shared @openstatus/db plan schema module before exposing creation endpoints.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# Discover and run the project test runner across all limit handler test suites
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>