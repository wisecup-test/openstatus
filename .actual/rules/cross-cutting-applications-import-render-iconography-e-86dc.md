---
version: 2
name: cross-cutting-applications-import-render-iconography-e-86dc
description: "Adoption of @openstatus/icons for Application Iconography: Applications Import Render Iconography Exclusively Openstatus"
metadata:
  actual:
    adr_id: 86dcd1f5-5b2d-4fe7-be25-e81a65956f7b
    status: accepted
    candidate_group_id: 189c3723-9793-4c50-9b60-a1dd4ce669b2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/icons for Application Iconography: Applications Import Render Iconography Exclusively Openstatus

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`, `apps/status-page/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OP-001** MUST: Applications MUST import and render iconography exclusively from @openstatus/icons for visual indicators, actions, and status representations.

### Verify

```bash
# Discover and run workspace static analysis, linting, type checking, and dependency audit scripts from the dependency manifest
# Example discovery:
# npm run lint / yarn lint / pnpm lint
# npm run typecheck / yarn typecheck / pnpm typecheck
# npm audit / pnpm audit
```

**Accept when:**
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, peer code reviews, and dependency checks will block unauthorized icon imports or inline SVG declarations.
</enforcement>