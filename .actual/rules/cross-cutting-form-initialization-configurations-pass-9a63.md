---
version: 2
name: cross-cutting-form-initialization-configurations-pass-9a63
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Form Initialization Configurations Pass Validation Schema"
metadata:
  actual:
    adr_id: 9a63f2e5-4d96-42ed-bc53-53ad62807bfa
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Form Initialization Configurations Pass Validation Schema

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all interactive client-side components capturing user input within form structures, dialogs, settings panels, and notification configuration interfaces requiring field-level data validation.

### Rules

- **R-FORM-001** MUST: Form initialization configurations MUST pass the validation schema directly to zodResolver within the resolver option of the form controller hook.

### Verify

```bash
# Discover and run static analysis and linting tasks
# Discover and execute workspace type-checking
# Discover and run automated test suites covering form component validation
```

**Accept when:**
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, type-checking tasks, and peer code review. Pull requests containing manual validation logic in place of the standard resolver MUST be blocked.
</enforcement>