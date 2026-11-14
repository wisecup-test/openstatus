---
version: 2
name: cross-cutting-components-within-package-consume-shared-153d
description: "Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Components Within Package Consume Shared Styling"
metadata:
  actual:
    adr_id: 153d0827-e218-434d-80c6-9409d97b4859
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

# Adoption of Internal Utility Module (@openstatus/ui/lib/utils) in UI Components: Components Within Package Consume Shared Styling

Scope: applies to `packages/ui/src/components/**`, `packages/ui/src/hooks/**`, `packages/ui/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Components within the UI package MUST consume shared styling, class merging, and helper routines through the internal utility module (@openstatus/ui/lib/utils).

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts
# Discover and execute the package build and type verification scripts
npm run lint || pnpm lint || yarn lint
npm run build || pnpm build || yarn build
```

**Accept when:**
- All primitive and composite UI components in the shared UI package resolve utility logic via the internal shared library module.
- No direct imports of private or external utility implementations bypass the designated internal utility module.
- Automated static checks and build verification pass without unresolved workspace module references.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new and modified UI component implementations within the shared UI package must conform to centralized internal utility imports, verified via static analysis, automated package build, type-checking, and peer code review.
</enforcement>