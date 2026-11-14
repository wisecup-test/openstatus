---
version: 2
name: cross-cutting-client-form-components-requiring-input-4351
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Requiring Input Validation"
metadata:
  actual:
    adr_id: 43516136-9ab4-49fa-a96e-430acb27242f
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Requiring Input Validation

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all interactive client-side components capturing user input within form structures, including dialogs, settings panels, and notification configuration interfaces requiring field-level data validation.

### Rules

- **R-ZOD-001** MUST: Client form components requiring input validation MUST integrate @hookform/resolvers/zod using zodResolver to bind form controllers to declarative validation schemas.

### Verify

```bash
# Discover and run the project static analysis and linting tasks to verify form implementations
npm run lint
# Discover and execute the workspace type-checking command to ensure form controller types match schema definitions
npm run typecheck
# Discover and run automated test suites covering form component validation and error handling
npm run test
```

**Accept when:**
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>