---
version: 2
name: cross-cutting-shared-domain-operations-business-workfl-3c19
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Shared Domain Operations Business Workflows External"
metadata:
  actual:
    adr_id: 3c199616-597c-4b9a-98ec-5213371227f2
    status: accepted
    candidate_group_id: d7fa39ec-eb90-4ffa-b65a-12044e6ce4ef
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/services Module for Centralized Domain Logic: Shared Domain Operations Business Workflows External

Scope: applies to `apps/dashboard/src/app/api/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for application route handlers, edge context resolvers, webhook consumers, agent tooling, and shared domain logic layers.

### Rules

- **R-DOMAIN-001** MUST: All shared domain operations, business workflows, and external provider orchestrations MUST be consumed through the @openstatus/services package or its domain-scoped subpath entry points rather than re-implemented within transport or integration layers.

### Verify

```bash
# Discover workspace dependency manifest and run type verification
# Discover and run project test execution script targeting domain service modules
# Discover and execute lint and module boundary validation suite
```

**Accept when:**
- Static type verification succeeds across all consumers of @openstatus/services without interface mismatches.
- All unit and integration test suites testing domain services and consumer endpoints pass with zero failures.
- Architecture boundary checks verify that domain logic in route handlers and agent tools is invoked exclusively through @openstatus/services.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>