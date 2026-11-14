---
version: 2
name: cross-cutting-consumers-inspect-workspace-dependency-m-6689
description: "Adoption of @openstatus/regions for Regional Monitoring Services: Consumers Inspect Workspace Dependency Manifest Repository"
metadata:
  actual:
    adr_id: 6689a493-0eb9-43d4-95bb-7b80a169baf9
    status: accepted
    candidate_group_id: 7d6c947a-2ce9-4980-87e0-d71f41deed1f
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/regions for Regional Monitoring Services: Consumers Inspect Workspace Dependency Manifest Repository

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Consumers MUST inspect the workspace dependency manifest and repository resolution artifact to resolve the exact version of @openstatus/regions before incorporating its exports into service logic.

### Verify

```bash
# Discover and execute the project type-checking script from the repository manifest to verify @openstatus/regions import compliance across services
# Discover and run the project test suite from the repository manifest to validate regional header injection and preview dispatch handling
```

**Accept when:**
- Static analysis and type validation pass across all service modules importing @openstatus/regions without unresolved symbols or mismatched export contracts.
- Regional monitoring dispatch and stream preview tests pass, confirming standardized regional header propagation and timeout signal bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>