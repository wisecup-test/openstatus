---
version: 2
name: cross-cutting-before-implementing-upgrading-dependenci-b09d
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Before Implementing Upgrading Dependencies Engineering Team"
metadata:
  actual:
    adr_id: b09d463b-d29d-44ec-83ca-8ac1b5d12495
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Before Implementing Upgrading Dependencies Engineering Team

Scope: applies to `apps/status-page/src/components/forms/**`.

These rules are ALWAYS ACTIVE for interactive client-rendered form components that manage user input state and require declarative schema validation, and form workflows requiring pre-submission client-side validation and structured field error rendering.

### Rules

- **R-FORM-001** MUST: Before implementing or upgrading dependencies, the engineering team MUST discover the repository lock artifact and verify the exact resolved version of @hookform/resolvers/zod against official documentation.
- **R-FORM-002** MUST: Follow the lock-version grounding process: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version before using it, and re-run for version-sensitive behavior at point of use.
- **R-FORM-003** MUST: Configure form state controllers by passing the resolver instance initialized with the component validation schema during form initialization.
- **R-FORM-004** MUST: Ensure all interactive form inputs connect their registered field handlers and error states directly to the form controller managed by the resolver.

### Verify

```bash
# Discover and run the project static analysis and type checking suite
# Discover and execute the project automated test suite covering interactive form components and validation workflows
```

**Accept when:**
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>