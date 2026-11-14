---
version: 2
name: cross-cutting-route-handlers-supply-defensive-fallback-e93b
description: "Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Supply Defensive Fallback Values"
metadata:
  actual:
    adr_id: e93b54de-adae-489e-87be-18f724ace008
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Route Handlers Supply Defensive Fallback Values

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Route handlers MUST supply defensive fallback values when requested parameter values are missing or invalid.

### Verify

```bash
# Run the repository validation script to confirm type checking and build integrity
# Run the test suite discovered from the package configuration to verify badge route handler responses and parameter resolution
```

**Accept when:**
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>