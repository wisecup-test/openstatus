---
version: 2
name: cross-cutting-before-writing-code-class-variance-9821
description: "Adoption of class-variance-authority for Component Variant Styling: Before Writing Code Class Variance Authority"
metadata:
  actual:
    adr_id: 9821a60d-996a-41a8-a0fb-a4a5ce859bd2
    status: accepted
    candidate_group_id: e46753c3-3ce4-4422-991e-d39a1edcb3cc
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
---

# Adoption of class-variance-authority for Component Variant Styling: Before Writing Code Class Variance Authority

Scope: applies to `apps/dashboard/src/components/**`.

These rules are ALWAYS ACTIVE for presentation and UI component declarations requiring dynamic styling variants or state-based styling.

### Rules

- **R-CVA-001** MUST: Before writing code using class-variance-authority, developers MUST discover the project dependency lock artifact and resolve the exact locked version to ensure API compatibility.

### Verify

```bash
discover_repository_linter && execute_linter
discover_repository_typechecker && execute_typecheck
discover_repository_test_runner && execute_component_tests
```

**Accept when:**
- All component variant definitions compile without type diagnostics under the project typechecker.
- Component unit and visual regression tests pass without class resolution errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>