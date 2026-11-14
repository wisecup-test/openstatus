---
version: 2
name: cross-cutting-public-status-badge-route-handlers-b585
description: "Adoption of @openstatus/react for Status Badge Rendering: Public Status Badge Route Handlers Use"
metadata:
  actual:
    adr_id: b5851c60-2cc1-4219-b52d-c80db844dff1
    status: accepted
    candidate_group_id: 246e0c99-70ef-484e-8541-0f996fcd3284
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/react for Status Badge Rendering: Public Status Badge Route Handlers Use

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Public status badge route handlers MUST use @openstatus/react as the authoritative presentation component library for rendering status badges.

### Verify

```bash
# Find and execute the repository validation script defined in the project configuration to confirm type checking and build integrity for badge route handlers.
# Run the test suite discovered from the package configuration to verify badge route handler responses and parameter resolution.
```

**Accept when:**
- The repository test suite passes for all status badge route endpoints without regressions.
- All badge rendering routes resolve components exclusively from @openstatus/react without local styling duplication.
- Dependency resolution confirms that the installed version of @openstatus/react matches the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>