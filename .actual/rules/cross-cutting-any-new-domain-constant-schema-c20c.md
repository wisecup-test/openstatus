---
version: 2
name: cross-cutting-any-new-domain-constant-schema-c20c
description: "Adoption of @openstatus/db Schema Constants Module: Any New Domain Constant Schema Boundary"
metadata:
  actual:
    adr_id: c20c7dbd-ec62-45a4-aa57-bc7294bce645
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Any New Domain Constant Schema Boundary

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Any new domain constant or schema boundary value required across service boundaries MUST be declared within @openstatus/db and exported through its schema constants module before consumption in service routes.

### Verify

```bash
# Discover the primary package configuration script for static type verification and execute it.
# Discover the workspace test execution script and run the automated test suite against consuming packages.
# Inspect the repository script definitions and execute the linter to verify import boundary constraints.
```

**Accept when:**
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline executing repository verification scripts and peer code review verifying adherence to centralized schema constant imports.
</enforcement>