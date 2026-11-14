---
version: 2
name: cross-cutting-form-components-propagate-validation-err-646f
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Form Components Propagate Validation Errors Produced"
metadata:
  actual:
    adr_id: 646fbf5f-c4b7-43a5-8ee3-8007947d0463
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Form Components Propagate Validation Errors Produced

Scope: applies to `apps/status-page/src/components/forms/**`.

These rules are ALWAYS ACTIVE for interactive client-rendered form components that manage user input state and require declarative schema validation, and form workflows requiring pre-submission client-side validation and structured field error rendering.

### Rules

- **R-HFR-001** SHOULD: Form components SHOULD propagate validation errors produced by @hookform/resolvers/zod directly to UI field presentation elements to provide inline feedback.

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