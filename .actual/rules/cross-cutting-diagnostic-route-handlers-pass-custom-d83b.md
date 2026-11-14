---
version: 2
name: cross-cutting-diagnostic-route-handlers-pass-custom-d83b
description: "Adoption of @openstatus/regions for Unified Geographic Region Modeling: Diagnostic Route Handlers Pass Custom Probe"
metadata:
  actual:
    adr_id: d83b53f5-af3e-432c-9d9d-4c996363835e
    status: accepted
    candidate_group_id: 903339f8-5d24-42ec-828d-2745bb17f92b
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/data/**
      - src/lib/checker/**
---

# Adoption of @openstatus/regions for Unified Geographic Region Modeling: Diagnostic Route Handlers Pass Custom Probe

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/data/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MAY: Diagnostic route handlers MAY pass custom probe configuration along with the canonical region identifier when delegating checks to regional edge workers.

### Verify

```bash
# Discover and execute the test suite covering regional schema validation and probe routing from the dependency manifest
# Discover and execute type inspection and static analysis scripts to verify region imports resolve without error
```

**Accept when:**
- All test suites validating regional latency checks, probe routing, and payload parsing complete with zero schema validation errors.
- Static analysis and type validation confirm that all regional attributes and lookup functions resolve directly to @openstatus/regions without unresolved symbol errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests defining local region string unions or ad-hoc region schemas are blocked by automated review gates, and static analysis errors must be resolved by replacing local definitions with imports from the shared module before merging.
</enforcement>