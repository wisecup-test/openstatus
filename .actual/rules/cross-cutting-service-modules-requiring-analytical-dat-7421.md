---
version: 2
name: cross-cutting-service-modules-requiring-analytical-dat-7421
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Service Modules Requiring Analytical Data Access"
metadata:
  actual:
    adr_id: 74214043-8a5e-4896-b150-4b7beb3338fb
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Service Modules Requiring Analytical Data Access

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service modules executing analytical telemetry queries and aggregations, and context modules managing analytics client initialization and credentials.

### Rules

- **R-ANALYTICS-001** MUST: Service modules requiring analytical data access MUST consume @openstatus/tinybird rather than executing raw external analytics network calls directly.

### Verify

```bash
# Discover the project test runner and execute the automated test suite for service context and monitor modules
# Discover the workspace static analysis and type-checking scripts to verify correct usage of @openstatus/tinybird imports
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>