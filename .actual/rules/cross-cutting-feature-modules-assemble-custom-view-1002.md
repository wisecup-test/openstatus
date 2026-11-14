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
- **R-UI-002** MANDATORY: When introducing new user interface elements, verify whether existing primitives or composite blocks within @openstatus/ui meet presentation requirements prior to authoring custom structures.
- **R-UI-003** MANDATORY: Ensure all styling merges and dynamic class assignments utilize helper routines imported directly from @openstatus/ui/lib/utils.
- **R-UI-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest; (2) Identify build tool; (3) Inspect repository lock/resolution artifact; (4) Look up official documentation/changelog for that exact version; (5) Confirm every API/class/function exists in that version's docs; (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute workspace static analysis and type verification across all frontend packages
# Discover and execute test runner for component test suite
# Discover and execute repository import boundary and linting check command
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated static analysis, linting passes, type checking, and peer code reviews enforce compliance.
</enforcement>