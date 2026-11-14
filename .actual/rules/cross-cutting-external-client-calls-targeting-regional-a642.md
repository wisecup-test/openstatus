---
version: 2
name: cross-cutting-external-client-calls-targeting-regional-a642
description: "Adoption of @openstatus/regions for Regional Monitoring Services: External Client Calls Targeting Regional Endpoints"
metadata:
  actual:
    adr_id: a642fefb-2a5b-45cb-be6b-159895ee9a03
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: External Client Calls Targeting Regional Endpoints

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** SHOULD: External client calls targeting regional endpoints SHOULD apply regional routing headers and standard regional timeout signals provided by @openstatus/regions conventions.

### Verify

```bash
# Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services.
# Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling.
```

**Accept when:**
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>