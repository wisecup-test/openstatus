---
version: 2
name: cross-cutting-validation-schemas-passed-hookform-resol-83dc
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Validation Schemas Passed Hookform Resolvers Zod"
metadata:
  actual:
    adr_id: 83dcda93-793f-45a1-b019-e9bc3a134356
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Validation Schemas Passed Hookform Resolvers Zod

Scope: applies to `apps/status-page/src/components/forms/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VAL-001** MUST: Validation schemas passed to @hookform/resolvers/zod MUST define explicit type constraints for all form input fields prior to handling user submission events.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify resolver integration across form components.
# Discover and execute the project automated test suite covering interactive form components and validation workflows.
```

**Accept when:**
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>