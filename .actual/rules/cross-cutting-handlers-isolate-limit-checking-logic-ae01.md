---
version: 2
name: cross-cutting-handlers-isolate-limit-checking-logic-ae01
description: "Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Handlers Isolate Limit Checking Logic Dedicated"
metadata:
  actual:
    adr_id: ae01e455-802f-4cb5-8967-fc0439748c05
    status: accepted
    candidate_group_id: 7ae5dac6-a0e6-45e2-a8b7-8134be53ba90
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/handlers/**
---

# Adoption of @openstatus/db Plan Schema for Centralized RPC Limit Enforcement: Handlers Isolate Limit Checking Logic Dedicated

Scope: applies to `apps/server/src/routes/rpc/handlers/**`.

These rules are ALWAYS ACTIVE for Server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-RPC-001** SHOULD: Handlers isolate limit checking logic into dedicated limit handler units within the RPC route hierarchy to decouple quota validation from core business execution.
- **R-RPC-002** MUST: Consume all plan limit calculations and entitlement thresholds directly from the plan schema exports rather than derived through bespoke database queries or decentralized domain-level quota constants.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# (e.g., pnpm --filter <workspace> typecheck or similar discovered via project repository)
# Verify that RPC limit handlers import quota definitions solely from the canonical plan schema module

# Discover and run the project test runner across all limit handler test suites
# (e.g., pnpm test or similar discovered via project repository)
# Verify quota enforcement logic against defined plan schemas
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing decentralized quota constants or hardcoded limit definitions must be rejected or refactored to import canonical plan schema structures before merging.
</enforcement>