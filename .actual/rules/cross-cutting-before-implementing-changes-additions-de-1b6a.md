---
version: 2
name: cross-cutting-before-implementing-changes-additions-de-1b6a
description: "Adoption of @openstatus/services Module for Centralized Domain Logic: Before Implementing Changes Additions Depending Workspace"
metadata:
  actual:
    adr_id: 1b6ae784-e84d-4ea7-bea8-05b51bca2c26
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

# Adoption of @openstatus/services Module for Centralized Domain Logic: Before Implementing Changes Additions Depending Workspace

Scope: applies to `apps/dashboard/src/app/api/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing changes or additions depending on workspace packages or external modules, consumers MUST inspect the repository lock artifact to verify the exact resolved versions and manifest dependencies.
- **R-OPENSTATUS-002** MUST: Structure domain services with dedicated subpath exports to maintain clear boundary segregation between independent capabilities.
- **R-OPENSTATUS-003** MUST: Ensure all consumer calls to service functions pass structured parameters that have been validated at the integration boundary.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines validate static types, run unit/integration tests, and enforce module boundaries, blocking pull requests on failures.
</enforcement>