---
version: 2
name: cross-cutting-client-form-components-not-implement-0585
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Not Implement Custom"
metadata:
  actual:
    adr_id: 0585fd27-d6b3-42f5-9e56-255bafb94c77
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Client Form Components Not Implement Custom

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Client form components MUST NOT implement custom manual validation routines when input rules can be represented via schema validation resolvers.

### Verify

```bash
# Discover and run the project static analysis and linting tasks
# Discover and execute the workspace type-checking command
# Discover and run automated test suites covering form component validation
```

**Accept when:**
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>