---
version: 2
name: cross-cutting-new-icon-assets-intended-shared-457a
description: "Adoption of @openstatus/icons for Application Iconography: New Icon Assets Intended Shared Visual"
metadata:
  actual:
    adr_id: 457a4e99-920f-4c4f-b4d9-a69bfa63ab0d
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

# Adoption of @openstatus/icons for Application Iconography: New Icon Assets Intended Shared Visual

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`, `apps/status-page/src/data/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICN-001** SHOULD: New icon assets intended for shared visual indicators SHOULD be contributed directly to @openstatus/icons rather than declared as localized component assets.

### Verify

```bash
# Discover and run the workspace static analysis, linting, type checking, and dependency audit scripts from the dependency manifest.
```

**Accept when:**
- All client components and client data modules requiring iconography resolve their icon symbols exclusively from @openstatus/icons.
- Static verification and dependency analysis execute without reporting missing icon bindings or undeclared icon packages.
- The resolved package version in the dependency resolution artifact matches the verified interface signatures used across consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>