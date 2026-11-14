---
version: 2
name: cross-cutting-components-client-data-modules-not-757f
description: "Adoption of @openstatus/icons for Application Iconography: Components Client Data Modules Not Embed"
metadata:
  actual:
    adr_id: 757f5688-0708-40a9-aa9f-cb4e26bbd3c0
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

# Adoption of @openstatus/icons for Application Iconography: Components Client Data Modules Not Embed

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`, `apps/status-page/src/data/**`.

These rules are ALWAYS ACTIVE for all client-side user interface components rendering visual symbols, status badges, action controls, and client-side data definitions and descriptors.

### Rules

- **R-ICONS-001** MUST_NOT: UI components and client data modules MUST NOT embed raw SVG markup or import third-party icon libraries directly.

### Verify

```bash
# Discover and run the workspace static analysis, linting, type checking, and dependency audit commands from the manifest.
# Example discovery flow:
# 1. Read package.json / project manifests to find lint, typecheck, and audit scripts.
# 2. Run the lint script to verify all icon imports originate from @openstatus/icons.
# 3. Run the type-checking script to validate icon component properties and imported symbols.
# 4. Run the dependency audit script to confirm no unauthorized external icon packages are declared.
```

**Accept when:**
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, peer code review, and dependency verification checks enforce that pull requests containing inline SVG declarations or unauthorized icon imports are blocked from merging.
</enforcement>