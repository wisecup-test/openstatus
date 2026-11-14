---
version: 2
name: cross-cutting-frontend-modules-not-implement-bespoke-29ee
description: "Adoption of @openstatus/ui Shared Component Library: Frontend Modules Not Implement Bespoke Local"
metadata:
  actual:
    adr_id: 29ee8820-51be-4c40-9b4b-3dd3f58fb8de
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

# Adoption of @openstatus/ui Shared Component Library: Frontend Modules Not Implement Bespoke Local

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST_NOT: Frontend modules MUST NOT implement bespoke local duplicates of primitives or utility helpers already provided by @openstatus/ui.

### Verify

```bash
# Discover and run static analysis and type verification script across frontend packages
# Discover and run the component test suite
# Discover and run repository import boundary and linting check command
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>