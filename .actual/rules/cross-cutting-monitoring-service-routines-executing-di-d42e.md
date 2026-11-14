---
version: 2
name: cross-cutting-monitoring-service-routines-executing-di-d42e
description: "Adoption of @openstatus/regions for Regional Monitoring Services: Monitoring Service Routines Executing Dispatching Region"
metadata:
  actual:
    adr_id: d42e0eb8-c325-4762-8809-4f7bc003adb3
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: Monitoring Service Routines Executing Dispatching Region

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all monitoring domain service handlers, preview streaming routines, and internal database queries or service workflows that interface with regional probe endpoints or require geographic region metadata.

### Rules

- **R-REGION-001** MUST: Monitoring service routines executing or dispatching region-aware operations MUST import and utilize @openstatus/regions as the sole authority for regional metadata and request headers.

### Verify

```bash
# Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services.
# Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling.
```

**Accept when:**
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, type checking, and test suites.
</enforcement>