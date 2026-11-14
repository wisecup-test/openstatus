---
version: 2
name: cross-cutting-before-introducing-modifying-versioned-d-11dd
description: "Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Before Introducing Modifying Versioned Dependencies Supporting"
metadata:
  actual:
    adr_id: 11dda26e-6964-4724-b71d-2fae95d371e7
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

# Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Before Introducing Modifying Versioned Dependencies Supporting

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Before introducing or modifying versioned dependencies supporting the utility module, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency versions.
- **R-UTIL-002** MUST: Ensure that all new component implementations inside the UI package import class composition and styling helpers exclusively from the designated internal utility module.
- **R-UTIL-003** SHOULD: Avoid exporting component-specific business logic from the shared utility module; keep it focused on cross-cutting helper functions.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify compliance with internal utility import paths.
# Discover and execute the package build and type verification scripts to confirm all utility module references resolve cleanly.
```

**Accept when:**
- All primitive and composite UI components in the shared UI package resolve utility logic via the internal shared library module.
- No direct imports of private or external utility implementations bypass the designated internal utility module.
- Automated static checks and build verification pass without unresolved workspace module references.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by static analysis, type checking, and automated build steps is mandatory.
</enforcement>