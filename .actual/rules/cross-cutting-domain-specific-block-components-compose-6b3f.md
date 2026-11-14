---
version: 2
name: cross-cutting-domain-specific-block-components-compose-6b3f
description: "Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Domain Specific Block Components Compose Lower"
metadata:
  actual:
    adr_id: 6b3f83c8-08b3-48eb-a9b8-e089dfade65f
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

# Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Domain Specific Block Components Compose Lower

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all component, block, and hook implementations located within the shared UI package, as well as shared formatting, styling, and general utility helper functions consumed by UI components.

### Rules

- **R-UTIL-001** SHOULD: Domain-specific block components SHOULD compose lower-level UI primitives and utility modules rather than creating ad-hoc styling abstraction layers.
- **R-UTIL-002** MUST: Ensure that all new component implementations inside the UI package import class composition and styling helpers exclusively from the designated internal utility module.
- **R-UTIL-003** MUST: Avoid exporting component-specific business logic from the shared utility module; keep it focused on cross-cutting helper functions.

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
Claude Code MUST NOT skip or defer verification. All pull requests containing duplicate helper routines or direct third-party utility bypasses must be blocked, and violations identified in automated checks must be refactored before merging.
</enforcement>