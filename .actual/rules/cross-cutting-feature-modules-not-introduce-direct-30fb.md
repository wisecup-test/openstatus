---
version: 2
name: cross-cutting-feature-modules-not-introduce-direct-30fb
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Feature Modules Not Introduce Direct Dependencies"
metadata:
  actual:
    adr_id: 30fbd7c3-b5eb-4c6f-acda-ab27eb247044
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

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Feature Modules Not Introduce Direct Dependencies

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** SHOULD_NOT: Feature modules SHOULD NOT introduce direct dependencies on external third-party iconography packages, routing asset access through the shared iconography workspace package.

### Verify

```bash
# Discover and execute repository static analysis and type checking suite
npm run typecheck

# Discover and execute repository linting rules to validate imports
npm run lint

# Discover and run the project build scripts to confirm clean module bundling
npm run build
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All icon dependencies and imports must comply strictly with the shared iconography workspace packages.
</enforcement>