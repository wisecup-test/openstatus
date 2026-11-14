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

These rules are ALWAYS ACTIVE for all user interface views and presentation components requiring graphical icons, action symbols, or visual indicators, including configurations, notification settings, and data table columns presenting external service logos and brand marks.

### Rules

- **R-ICONS-001** MUST_NOT: Presentation components MUST NOT define ad-hoc inline vector paths or duplicate scalable vector graphics directly in component source files.
- **R-ICONS-002** MUST: Ensure icon component imports explicitly distinguish between general UI symbols and third-party provider marks from the designated subpath.
- **R-ICONS-003** MUST: Validate that stroke weights, viewports, and color inheritance attributes conform to existing workspace icon definitions when introducing new visual marks.

### Verify

```bash
# Discover and execute the repository static analysis and type checking suite
# Discover and execute repository linting rules to validate no ad-hoc third-party icons or duplicate vectors exist
# Discover and run project build scripts to confirm clean module bundling and tree-shaking
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through the approved iconography packages or subpaths.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying UI presentation layers must pass automated static analysis, type checking, lint rules, and peer code reviews ensuring adherence to centralized iconography.
</enforcement>