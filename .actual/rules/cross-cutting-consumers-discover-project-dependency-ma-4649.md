---
version: 2
name: cross-cutting-consumers-discover-project-dependency-ma-4649
description: "Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Consumers Discover Project Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 464993ae-ed10-44f6-ba38-b661c757493a
    status: accepted
    candidate_group_id: c76274ed-0898-4dbb-99b0-5dffa694bfd9
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of @hookform/resolvers/zod for Schema-Driven Form Validation: Consumers Discover Project Dependency Manifest Authoritative

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for all client-side interactive components capturing user input within form structures, dialogs, settings panels, and notification configuration interfaces requiring field-level data validation.

### Rules

- **R-ZOD-001** MUST: Consumers MUST discover the project dependency manifest and authoritative resolution lock artifact to verify the exact resolved version of @hookform/resolvers/zod before authoring form logic.

### Verify

```bash
# Discover and run static analysis, linting, and type-checking tasks
# Example discovery-based commands:
# 1. Locate and run project lint/static analysis scripts for form implementations
# 2. Run workspace type-checking to verify form controller types match schema definitions
# 3. Execute automated test suites covering form component validation and error handling
```

**Accept when:**
- All client form components requiring input validation import and configure zodResolver from @hookform/resolvers/zod.
- Project type checking and static verification scripts execute with zero resolver-related schema type errors.
- Automated component test suites verify that invalid inputs correctly trigger schema-defined validation errors through the resolver mechanism.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, code review checks during CI, repository type-checking tasks, and peer code reviews. Pull requests containing manual validation logic in place of the standard resolver MUST be blocked.
</enforcement>