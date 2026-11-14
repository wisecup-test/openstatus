---
version: 2
name: cross-cutting-consumers-import-domain-capabilities-spe-1654
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Consumers Import Domain Capabilities Specific Subpath"
metadata:
  actual:
    adr_id: 165479bb-1d33-45fe-8970-801b0a39e7b9
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

# Adoption of @openstatus/services Module for Centralized Domain Logic: Consumers Import Domain Capabilities Specific Subpath

Scope: applies to `apps/dashboard/src/app/api/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** SHOULD: Consumers SHOULD import domain capabilities from specific subpath entry points when available within @openstatus/services to enforce domain boundary isolation.

### Verify

```bash
# Discover the workspace dependency manifest and run the repository type verification script
# Discover and run the project test execution script targeting domain service modules and consuming endpoints
# Discover and execute the lint and module boundary validation suite
```

**Accept when:**
- Static type verification succeeds across all consumers of @openstatus/services without interface mismatches.
- All unit and integration test suites testing domain services and consumer endpoints pass with zero failures.
- Architecture boundary checks verify that domain logic in route handlers and agent tools is invoked exclusively through @openstatus/services.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>