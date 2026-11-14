---
version: 2
name: cross-cutting-frontend-presentation-modules-consume-vi-3fe6
description: "Adoption of @openstatus/ui Shared Component Library: Frontend Presentation Modules Consume Visual Primitives"
metadata:
  actual:
    adr_id: 3fe686b7-b0b9-410d-ab04-6f5110448a30
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

# Adoption of @openstatus/ui Shared Component Library: Frontend Presentation Modules Consume Visual Primitives

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: Frontend presentation modules MUST consume visual primitives, compound blocks, and shared styling helpers from @openstatus/ui.

### Verify

```bash
# Discover and run the workspace static analysis and type verification script across all frontend packages.
# Discover and run the test runner execution script defined in the project configuration.
# Discover and run the repository import boundary and linting check command to verify compliance with module import paths.
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing local duplicate primitives or improper import paths are blocked from merging until corrected.
</enforcement>