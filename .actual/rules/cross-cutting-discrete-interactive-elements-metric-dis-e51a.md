---
version: 2
name: cross-cutting-discrete-interactive-elements-metric-dis-e51a
description: "Adoption of @openstatus/ui Shared Component Library: Discrete Interactive Elements Metric Displays Source"
metadata:
  actual:
    adr_id: e51ad8c9-aa84-4713-a668-239a1414428a
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

# Adoption of @openstatus/ui Shared Component Library: Discrete Interactive Elements Metric Displays Source

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** SHOULD: Discrete interactive elements and metric displays SHOULD source underlying primitives from @openstatus/ui/components/ui.
- **R-UI-002** MANDATORY: When introducing new user interface elements, verify whether existing primitives or composite blocks within @openstatus/ui meet presentation requirements prior to authoring custom structures.
- **R-UI-003** MANDATORY: Ensure all styling merges and dynamic class assignments utilize helper routines imported directly from @openstatus/ui/lib/utils.

### Verify

```bash
# Discover and run workspace static analysis and type verification across all frontend packages
# Discover and execute component test suite via project configuration
# Discover and execute repository import boundary and linting check command
```

**Accept when:**
- Static type analysis and linting checks pass with zero import errors or schema mismatches across all consuming views.
- All UI components correctly resolve and render their designated @openstatus/ui primitives and compound blocks without runtime errors.
- No forbidden local duplicate UI primitives or direct styling helper definitions are detected in application code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration automated static analysis, linting passes, type checking, and peer code review verification.
</enforcement>