---
version: 2
name: cross-cutting-instantiation-execution-parameters-opens-c2a9
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Instantiation Execution Parameters Openstatus Tinybird Supplied"
metadata:
  actual:
    adr_id: c2a9c9a8-1a1f-49f3-bb68-ef87276be0c9
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Instantiation Execution Parameters Openstatus Tinybird Supplied

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service modules executing analytical telemetry queries and context modules managing analytics client initialization and credentials.

### Rules

- **R-TB-001** MUST: Instantiation and execution parameters for @openstatus/tinybird MUST be supplied through centralized service context utilizing runtime configuration variables TINY_BIRD_API_KEY, TINYBIRD_URL, and TINYBIRD_NOOP.

### Verify

```bash
# Discover and run the project test runner for service context and monitor modules
# Discover and run workspace static analysis and type-checking scripts
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites, static analysis checks, and peer code reviews enforce compliance.
</enforcement>