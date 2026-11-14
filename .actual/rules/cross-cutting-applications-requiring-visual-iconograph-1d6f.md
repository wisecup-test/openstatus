---
version: 2
name: cross-cutting-applications-requiring-visual-iconograph-1d6f
description: "Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Applications Requiring Visual Iconography Third Party"
metadata:
  actual:
    adr_id: 1d6f9944-3e4c-40cb-b684-38cb25024328
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

# Adoption of @openstatus/icons and Brand Subpath for Centralized Iconography: Applications Requiring Visual Iconography Third Party

Scope: applies to `apps/dashboard/src/app/login/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/config/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST: Applications requiring visual iconography or third-party brand imagery MUST consume graphical symbols exclusively from @openstatus/icons and @openstatus/icons/brand.

### Verify

```bash
# Discover and execute static analysis, type checking, linting, and build scripts
# (Commands to be discovered from package.json manifest)
npm run check || yarn check || pnpm check
npm run lint || yarn lint || pnpm lint
npm run build || yarn build || pnpm build
```

**Accept when:**
- All graphical icons and provider brand marks are imported exclusively through @openstatus/icons or @openstatus/icons/brand.
- Static analysis and type verification confirm clean symbol resolution across all user interface modules.
- Build artifact inspection verifies that icon subpaths bundle cleanly without duplicate vector definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, linting, and build verification must confirm that all icon imports use @openstatus/icons or @openstatus/icons/brand and that no inline vector assets or unapproved icon libraries are present.
</enforcement>