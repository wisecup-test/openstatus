---
version: 2
name: cross-cutting-client-rendered-form-components-requirin-abdf
description: "Adoption of @hookform/resolvers/zod for Form Validation Resolution: Client Rendered Form Components Requiring Structured"
metadata:
  actual:
    adr_id: abdf720b-48f9-4356-a7f1-e0490bc7ea42
    status: accepted
    candidate_group_id: e6e31cf3-078f-46c3-b58b-103d9e11fd5d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/forms/**
---

# Adoption of @hookform/resolvers/zod for Form Validation Resolution: Client Rendered Form Components Requiring Structured

Scope: applies to `apps/status-page/src/components/forms/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-FORM-001** MUST: All client-rendered form components requiring structured input validation MUST integrate @hookform/resolvers/zod to resolve validation schemas within form controller definitions.

### Verify

```bash
# Discover and run the project static analysis and type checking suite
# Discover and execute the project automated test suite covering interactive form components and validation workflows
```

**Accept when:**
- All client form components pass static type checking and compile without resolver type mismatch errors.
- Automated form test suites demonstrate that invalid user inputs trigger expected validation messages and halt submission.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipeline static analysis and automated test verification suites, and peer code review verification for all new and modified client form components.
</enforcement>