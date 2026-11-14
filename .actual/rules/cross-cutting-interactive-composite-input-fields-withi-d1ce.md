---
version: 2
name: cross-cutting-interactive-composite-input-fields-withi-d1ce
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Interactive Composite Input Fields Within Client"
metadata:
  actual:
    adr_id: d1cea9af-62c6-4124-a096-6d93f93aa5ab
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Interactive Composite Input Fields Within Client

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-INPUT-001** MUST: Interactive composite input fields within client-rendered dashboard components MUST import and utilize the internal @openstatus/ui/components/ui/input-group module for grouped control layouts.

### Verify

```bash
# Discover and run the project repository type verification suite to validate module import paths and interface contracts.
# Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules.
```

**Accept when:**
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. All composite input fields in client components must use the designated input group module.
</enforcement>