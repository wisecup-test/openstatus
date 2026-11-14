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

These rules are ALWAYS ACTIVE for all component, block, and hook implementations located within the shared UI package, and shared formatting, styling, and general utility helper functions consumed by UI components.

### Rules

- **R-UTIL-001** MUST: Before introducing or modifying versioned dependencies supporting the utility module, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency versions.
- **R-UTIL-002** MUST: Ensure that all new component implementations inside the UI package import class composition and styling helpers exclusively from the designated internal utility module (@openstatus/ui/lib/utils).
- **R-UTIL-003** SHOULD: Avoid exporting component-specific business logic from the shared utility module; keep it focused on cross-cutting helper functions.

### Verify

```bash
# Discover and execute repository static analysis, linting, package build, and type verification scripts
# Example discovery pattern:
# npm run lint || yarn lint || pnpm lint
# npm run build || yarn build || pnpm build
```

**Accept when:**
- All primitive and composite UI components in the shared UI package resolve utility logic via the internal shared library module.
- No direct imports of private or external utility implementations bypass the designated internal utility module.
- Automated static checks and build verification pass without unresolved workspace module references.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing duplicate helper routines or direct third-party utility bypasses must be blocked, and violations identified in automated checks must be refactored to consume the internal utility module before merging.
</enforcement>