---
version: 2
name: cross-cutting-form-components-rely-resolver-generated-a5f5
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Form Components Rely Resolver Generated Error"
metadata:
  actual:
    adr_id: a5f5eca4-b3ea-4c5f-bd55-215f1c6645e2
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Form Components Rely Resolver Generated Error

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all interactive client-side components capturing user input within form structures, dialogs, settings panels, and notification configuration interfaces.

### Rules

- **R-FORM-001** SHOULD: Form components SHOULD rely on resolver-generated error states for field-level feedback rather than maintaining independent error state stores.

### Verify

```bash
# Discover and run the project static analysis and linting tasks to verify form implementations
# Discover and execute the workspace type-checking command to ensure form controller types match schema definitions
# Discover and run automated test suites covering form component validation and error handling
```

**Accept when:**
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, type checking, and test suites during continuous integration and code review.
</enforcement>