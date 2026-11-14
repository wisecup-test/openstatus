---
version: 2
name: cross-cutting-service-operations-utilizing-openstatus-05b9
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Service Operations Utilizing Openstatus Tinybird Check"
metadata:
  actual:
    adr_id: 05b91fd6-11ac-47a4-94bc-95aa6c89d551
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Service Operations Utilizing Openstatus Tinybird Check

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TB-001** SHOULD: Service operations utilizing @openstatus/tinybird SHOULD check the operational no-op state configured via TINYBIRD_NOOP to support degraded or local execution modes.
- **R-TB-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-TB-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, re-run per dependency at point of use.
- **R-TB-004** MANDATORY: Construct analytical client instances exclusively within shared context factories to encapsulate secret ingestion and operational toggles.
- **R-TB-005** MANDATORY: Ensure query handlers validate input criteria prior to invoking analytics retrieval methods.

### Verify

```bash
# Discover the project test runner and execute the automated test suite for service context and monitor modules.
# Discover the workspace static analysis and type-checking scripts to verify correct usage of @openstatus/tinybird imports.
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, static analysis checks, and peer code reviews enforce compliance.
</enforcement>