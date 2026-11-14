---
version: 2
name: cross-cutting-service-modules-not-bypass-context-8379
description: "Adoption of @openstatus/tinybird Module for Service Analytics: Service Modules Not Bypass Context Read"
metadata:
  actual:
    adr_id: 8379f9d2-9ef0-4913-8340-32a454fa7d7b
    status: accepted
    candidate_group_id: a53674b5-d477-4fd3-baa8-2854157d1627
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Adoption of @openstatus/tinybird Module for Service Analytics: Service Modules Not Bypass Context Read

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service modules executing analytical telemetry queries and aggregations.

### Rules

- **R-ANALYTICS-001** MUST_NOT: Service modules MUST_NOT bypass service context to read analytical environment secrets or construct standalone client instances.

### Verify

```bash
# Discover and run test suite for service context and monitor modules
npm test
# Run workspace static analysis and type-checking scripts
npm run typecheck
```

**Accept when:**
- Static analysis and type verification pass without unresolved imports or unmanaged client instantiations.
- Context test suites verify proper credential mapping and graceful execution under the operational no-op flag.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via continuous integration automated test suites and static analysis checks.
</enforcement>