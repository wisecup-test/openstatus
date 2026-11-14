---
version: 2
name: cross-cutting-before-integrating-updating-dependencies-4aaf
description: "Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Before Integrating Updating Dependencies Internal External"
metadata:
  actual:
    adr_id: 4aaf8c64-b0e2-4e3b-bd6d-774553718aec
    status: accepted
    candidate_group_id: 9f73dff5-6372-4a30-8d58-fd257f35ff4e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @openstatus/ui/components/ui/input-group for Composite Input Fields: Before Integrating Updating Dependencies Internal External

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for client-rendered interactive user interface components requiring grouped input fields, action buttons, or inline adornments, form interfaces, and interactive prompt inputs within the dashboard application boundary.

### Rules

- **R-IG-001** MUST: Before integrating or updating dependencies from internal or external package namespaces, developers MUST discover the repository dependency manifest and resolution artifact to verify the locked version specifications.

### Verify

```bash
# Discover and run the project repository type verification suite to validate module import paths and interface contracts
# Discover and execute the repository linter and component validation scripts to check for compliance with design system import rules
```

**Accept when:**
- The repository type verification suite completes without errors regarding internal module imports.
- All composite input fields in client components resolve their layout structure through the designated input group module without local wrapper duplication.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>