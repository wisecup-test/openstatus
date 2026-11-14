---
version: 2
name: cross-cutting-feature-modules-assemble-custom-view-1002
description: "Adoption of @openstatus/ui Shared Component Library: Feature Modules Assemble Custom View Specific"
metadata:
  actual:
    adr_id: 10029272-2881-487f-88ed-a45c4c303938
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

# Adoption of @openstatus/ui Shared Component Library: Feature Modules Assemble Custom View Specific

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MAY: Feature modules MAY assemble custom view-specific compositions using @openstatus/ui primitives when existing composite blocks do not satisfy structural requirements.

### Verify

```bash
# Discover and execute the workspace static analysis and type verification script across all frontend packages.
# Discover and execute the test runner script defined in the project configuration.
# Discover and execute the repository import boundary and linting check command to verify compliance with module import paths.
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis, linting passes, and type checking enforce these rules.
</enforcement>