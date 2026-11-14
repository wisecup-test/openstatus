---
version: 2
name: cross-cutting-complex-multi-component-page-structures-375b
description: "Adoption of @openstatus/ui Shared Component Library: Complex Multi Component Page Structures Compose"
metadata:
  actual:
    adr_id: 375b48fb-8904-49ca-8d63-efaacb22e068
    status: accepted
    candidate_group_id: 31903934-689a-42b8-bdae-1bedc27d1b4a
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/components/**
---

# Adoption of @openstatus/ui Shared Component Library: Complex Multi Component Page Structures Compose

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** SHOULD: Complex multi-component page structures SHOULD compose from @openstatus/ui/components/blocks where available.

### Verify

```bash
# Discover and run the workspace static analysis and type verification script
pnpm typecheck || npm run typecheck

# Discover and run the component test suite
pnpm test || npm test

# Discover and run the repository import boundary and linting check command
pnpm lint || npm run lint
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis, linting passes, and type checking are mandatory.
</enforcement>