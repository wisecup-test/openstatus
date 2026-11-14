---
version: 2
name: cross-cutting-service-handlers-combine-analytical-quer-85f7
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Service Handlers Combine Analytical Query Results"
metadata:
  actual:
    adr_id: 85f740ff-ab7b-4d83-87b3-05d5d97b9d48
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Service Handlers Combine Analytical Query Results

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MAY: Service handlers MAY combine analytical query results from @openstatus/tinybird with relational entities obtained from @openstatus/db to construct composite reporting views.
- **R-ADR-002** MANDATORY: Construct analytical client instances exclusively within shared context factories to encapsulate secret ingestion and operational toggles.
- **R-ADR-003** MANDATORY: Ensure query handlers validate input criteria prior to invoking analytics retrieval methods.
- **R-ADR-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool. (3) Inspect the repository lock or resolution artifact. (4) Look up official documentation for that exact version. (5) Confirm every API, class, or function exists in that exact version. (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover and execute the project test runner for service context and monitor modules
# Discover workspace static analysis and type-checking scripts to verify correct usage of @openstatus/tinybird imports
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration automated test suites, static analysis checks, and peer code reviews. Violations result in automated pull request rejection.
</enforcement>