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

These rules are ALWAYS ACTIVE for server RPC route handlers that perform quota verification, tier entitlement checks, or resource boundary enforcement.

### Rules

- **R-RPC-001** SHOULD: Handlers SHOULD isolate limit checking logic into dedicated limit handler units within the RPC route hierarchy to decouple quota validation from core business execution.
- **R-RPC-002** MANDATORY: Structure quota evaluation logic into dedicated limit handler modules within each RPC feature domain to maintain modularity.
- **R-RPC-003** MANDATORY: Ensure all plan limit calculations and entitlement thresholds are consumed directly from the plan schema exports rather than derived through bespoke database queries.
- **R-RPC-004** MANDATORY: Before writing code that uses a versioned library, execute lock-version grounding steps in order: find dependency manifest, identify build tool, inspect lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use.

### Verify

```bash
# Discover and run the workspace type checker and static analysis script
# to verify that RPC limit handlers import quota definitions solely from the canonical plan schema module.
# (Discovered via repository configuration)
npm run typecheck
npm run lint

# Discover and run the project test runner across all limit handler test suites
# to verify quota enforcement logic against defined plan schemas.
npm test
```

**Accept when:**
- All RPC limit handler modules resolve plan definitions from the shared database schema without local quota overrides.
- Static analysis and test suites pass with zero type errors or schema import violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing decentralized quota constants or hardcoded limit definitions must be rejected. Code violating boundary rules must be refactored to import canonical plan schema structures before merging.
</enforcement>