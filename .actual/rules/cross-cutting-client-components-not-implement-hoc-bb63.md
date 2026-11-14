---
version: 2
name: cross-cutting-client-components-not-implement-hoc-bb63
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Client Components Not Implement Hoc Custom"
metadata:
  actual:
    adr_id: bb63d38d-5a45-4693-bd61-91cee5e4ef91
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Client Components Not Implement Hoc Custom

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-rendered interactive user interface components requiring grouped input fields, action buttons, or inline adornments, and form interfaces and interactive prompt inputs within the dashboard application boundary.

### Rules

- **R-INPUT-001** MUST_NOT: Client components MUST_NOT implement ad-hoc custom input wrapper containers or duplicated grouping styling when the standard input group module fulfills the required composite layout.

### Verify

```bash
# Discover and run the project repository type verification suite to validate module import paths and interface contracts.
# Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules.
```

**Accept when:**
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing ad-hoc input grouping containers or duplicate wrapper styling are blocked pending refactoring to the shared module.
</enforcement>