---
version: 2
name: cross-cutting-user-interface-components-rendering-exte-8822
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: User Interface Components Rendering External Service"
metadata:
  actual:
    adr_id: 88222455-248e-4637-8a06-43bb98396290
    status: accepted
    candidate_group_id: 6657c8b1-ac0f-40f5-afa9-ba814c964fbd
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/login/**
      - src/components/**
      - src/config/**
      - src/data/**
---

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: User Interface Components Rendering External Service

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST: User interface components rendering external service logos, provider badges, or third-party integration emblems MUST import those assets from @openstatus/icons/brand.

### Verify

```bash
# Discover and execute the repository static analysis and type checking suite
# Discover and execute repository linting rules to validate icon imports
# Discover and run the project build scripts to confirm clean module bundling
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified by automated static analysis, module import boundary checks, and peer code reviews.
</enforcement>