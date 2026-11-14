---
version: 2
name: cross-cutting-shared-class-merging-styling-utility-0533
description: "Adoption of @openstatus/ui Shared Component Library: Shared Class Merging Styling Utility Calls"
metadata:
  actual:
    adr_id: 0533dd7b-1552-4b2b-89d9-950ad3f513f6
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

# Adoption of @openstatus/ui Shared Component Library: Shared Class Merging Styling Utility Calls

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: Shared class-merging and styling utility calls MUST import directly from @openstatus/ui/lib/utils.

### Verify

```bash
# Discover and execute the workspace static analysis and type verification script across all frontend packages.
# Discover and execute the test runner execution script defined in the project configuration.
# Discover and execute the repository import boundary and linting check command to verify compliance with module import paths.
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>