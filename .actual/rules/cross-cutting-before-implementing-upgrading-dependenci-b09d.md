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

- **R-HOOK-001** MUST: Before implementing or upgrading dependencies, the engineering team MUST discover the repository lock artifact and verify the exact resolved version of @hookform/resolvers/zod against official documentation.

### Verify

```bash
# Discover and run the project static analysis, type checking suite, and automated test suite covering interactive form components and validation workflows.
# (Exact commands must be discovered from the project repository configuration)
```

**Accept when:**
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

<enforcement>
Verification is mandatory. Pull requests introducing manual validation logic or bypassing the schema resolver in form components must be blocked until compliant. Exceptions require documented architectural justification and formal approval by the frontend architecture lead.
</enforcement>