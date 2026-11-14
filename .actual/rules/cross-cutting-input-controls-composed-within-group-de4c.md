---
version: 2
name: cross-cutting-input-controls-composed-within-group-de4c
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Input Controls Composed Within Group Module"
metadata:
  actual:
    adr_id: de4cb6af-1d92-4194-ba45-8f97ebe02692
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Input Controls Composed Within Group Module

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-IG-001** SHOULD: Input controls composed within the input group module SHOULD integrate with standardized design system companion modules such as button and form field primitives from the same internal package hierarchy.

### Verify

```bash
# Discover and run the project repository type verification suite to validate module import paths and interface contracts.
# Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules.
```

**Accept when:**
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import linting, and peer code review enforce that ad-hoc input grouping containers or duplicate wrapper styling are blocked pending refactoring to the shared module.
</enforcement>