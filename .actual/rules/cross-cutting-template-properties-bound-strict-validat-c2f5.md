---
version: 2
name: cross-cutting-template-properties-bound-strict-validat-c2f5
description: "Adoption of react-email for Transactional Email Composition: Template Properties Bound Strict Validation Schemas"
metadata:
  actual:
    adr_id: c2f589b1-7bac-4904-8aee-62af5974cc83
    status: accepted
    candidate_group_id: b6d4a005-7d3f-4d2d-974c-51b7961ef7bc
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - emails/**
      - src/**
---

# Adoption of react-email for Transactional Email Composition: Template Properties Bound Strict Validation Schemas

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRAN-001** SHOULD: Template properties SHOULD be bound to strict validation schemas to guarantee data contract integrity prior to document rendering.

### Verify

```bash
# Discover and run type checking, test, and lint scripts from the project manifest
typecheck_cmd=$(npm pkg get scripts.typecheck 2>/dev/null | tr -d '"')
if [ -n "$typecheck_cmd" ] && [ "$typecheck_cmd" != "null" ]; then npm run typecheck; fi

test_cmd=$(npm pkg get scripts.test 2>/dev/null | tr -d '"')
if [ -n "$test_cmd" ] && [ "$test_cmd" != "null" ]; then npm run test; fi

lint_cmd=$(npm pkg get scripts.lint 2>/dev/null | tr -d '"')
if [ -n "$lint_cmd" ] && [ "$lint_cmd" != "null" ]; then npm run lint; fi
```

**Accept when:**
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>