---
version: 2
name: cross-cutting-backend-rpc-handlers-that-evaluate-ca5c
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Backend Rpc Handlers That Evaluate Resource"
metadata:
  actual:
    adr_id: ca5c384a-195d-46a9-8a0e-0aba5daa5392
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Backend Rpc Handlers That Evaluate Resource

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for all backend RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-DB-001** MUST: Backend RPC handlers that evaluate resource quotas or plan boundaries MUST import plan definitions exclusively from the @openstatus/db/src/schema/plan/schema module.

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