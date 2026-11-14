---
version: 2
name: cross-cutting-form-components-not-bypass-hookform-994c
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Form Components Not Bypass Hookform Resolvers"
metadata:
  actual:
    adr_id: 994c415a-662d-4d5c-b618-f1feaa91e5d8
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Form Components Not Bypass Hookform Resolvers

Scope: applies to `apps/status-page/src/components/forms/**`.

These rules are ALWAYS ACTIVE for interactive client-rendered form components that manage user input state and require declarative schema validation, and form workflows requiring pre-submission client-side validation and structured field error rendering.

### Rules

- **R-ZOD-001** MUST_NOT: Form components MUST NOT bypass @hookform/resolvers/zod in favor of ad-hoc, unvalidated manual form state inspection when structured validation schemas are defined.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify resolver integration across form components.
# Discover and execute the project automated test suite covering interactive form components and validation workflows.
```

**Accept when:**
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline static analysis and automated test verification suites enforce these rules, and pull requests introducing manual validation logic or bypassing the schema resolver in form components must be blocked until compliant.
</enforcement>