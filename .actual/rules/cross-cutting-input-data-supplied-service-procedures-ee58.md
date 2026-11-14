---
version: 2
name: cross-cutting-input-data-supplied-service-procedures-ee58
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Input Data Supplied Service Procedures Agent"
metadata:
  actual:
    adr_id: ee5825ea-c55e-4854-b4d8-6513549ecec8
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

# Adoption of @openstatus/services Module for Centralized Domain Logic: Input Data Supplied Service Procedures Agent

Scope: applies to `apps/dashboard/src/app/api/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for application route handlers, edge context resolvers, webhook consumers, and agent tooling consuming domain business capabilities, as well as shared domain logic, external service integrations, and business workflow orchestration.

### Rules

- **R-OPENSTATUS-001** MUST: Input data supplied to service procedures and agent adapters MUST undergo schema validation prior to invoking domain logic within @openstatus/services.

### Verify

```bash
# Discover workspace dependency manifest and run type verification
# Discover and run test execution script targeting domain service modules and consuming endpoints
# Discover and execute lint and module boundary validation suite
```

**Accept when:**
- Static type verification succeeds across all consumers of @openstatus/services without interface mismatches.
- All unit and integration test suites testing domain services and consumer endpoints pass with zero failures.
- Architecture boundary checks verify that domain logic in route handlers and agent tools is invoked exclusively through @openstatus/services.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>