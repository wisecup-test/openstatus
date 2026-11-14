---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-289b
description: "Adoption of @openstatus/ui Shared Component Library: Consumers Inspect Repository Dependency Manifest Resolution"
metadata:
  actual:
    adr_id: 289bd024-1643-4a51-9fcf-da3070dc50a7
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

# Adoption of @openstatus/ui Shared Component Library: Consumers Inspect Repository Dependency Manifest Resolution

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Consumers MUST inspect the repository dependency manifest and resolution artifact to resolve the exact dependency specification for @openstatus/ui before implementation.

### Verify

```bash
# Discover and run the workspace static analysis and type verification script
# Discover and run the test runner execution script defined in the project configuration
# Discover and run the repository import boundary and linting check command
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing local duplicate primitives or improper import paths are blocked from merging until corrected.
</enforcement>