---
version: 2
name: cross-cutting-presentation-components-not-define-hoc-ca6f
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Presentation Components Not Define Hoc Inline"
metadata:
  actual:
    adr_id: ca6f1c02-144f-4a36-b357-2b7ea2d7331c
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

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Presentation Components Not Define Hoc Inline

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all user interface views, presentation components, configurations, notification settings, and data table columns requiring graphical icons, action symbols, or visual indicators.

### Rules

- **R-ICON-001** MUST_NOT: Presentation components MUST NOT define ad-hoc inline vector paths or duplicate scalable vector graphics directly in component source files.

### Verify

```bash
# Discover and execute static analysis and type checking suite
# Discover and execute repository linting rules to validate no ad-hoc third-party icon libraries or duplicate vector paths
# Discover and run project build scripts to confirm clean module bundling and tree-shaking
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>