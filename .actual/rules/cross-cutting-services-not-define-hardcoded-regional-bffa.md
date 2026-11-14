---
version: 2
name: cross-cutting-services-not-define-hardcoded-regional-bffa
description: "Adoption of @openstatus/regions for Regional Monitoring Services: Services Not Define Hardcoded Regional Header"
metadata:
  actual:
    adr_id: bffa4cb9-2cb9-4f1d-abec-819f9c498ad6
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: Services Not Define Hardcoded Regional Header

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all monitoring domain service handlers, preview streaming routines interfacing with regional probe endpoints, internal database queries, and service workflows requiring geographic region metadata.

### Rules

- **R-REG-001** MUST_NOT: Services MUST NOT define hardcoded regional header strings or duplicate regional enum identifiers outside of @openstatus/regions.

### Verify

```bash
# Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services.
# Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling.
```

**Accept when:**
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification. All regional routing headers and configurations must originate from @openstatus/regions.
</enforcement>