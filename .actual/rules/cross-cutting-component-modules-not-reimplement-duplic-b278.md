---
version: 2
name: cross-cutting-component-modules-not-reimplement-duplic-b278
description: "Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Component Modules Not Reimplement Duplicate Helper"
metadata:
  actual:
    adr_id: b278079d-2308-432a-b11f-708b8575c6e4
    status: accepted
    candidate_group_id: 9ff50e4a-5fdd-4ce4-90ec-8cadf9e0c273
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/**
      - src/hooks/**
      - src/lib/**
---

# Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Component Modules Not Reimplement Duplicate Helper

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST_NOT: Component modules MUST NOT reimplement duplicate helper routines or directly bypass the internal utility module when equivalent shared utilities exist.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify compliance with internal utility import paths.
# Discover and execute the package build and type verification scripts to confirm all utility module references resolve cleanly.
npm run lint
npm run build
```

**Accept when:**
- All primitive and composite UI components in the shared UI package resolve utility logic via the internal shared library module.
- No direct imports of private or external utility implementations bypass the designated internal utility module.
- Automated static checks and build verification pass without unresolved workspace module references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis, linting, and package build steps.
</enforcement>