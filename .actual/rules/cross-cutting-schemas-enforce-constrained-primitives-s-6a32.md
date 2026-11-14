---
version: 2
name: cross-cutting-schemas-enforce-constrained-primitives-s-6a32
description: "Zod Schema Validation for Template Payloads: Schemas Enforce Constrained Primitives Such Enum"
metadata:
  actual:
    adr_id: 6a32e58b-9508-4999-b301-27fa761e51eb
    status: accepted
    candidate_group_id: 6f7dcbfa-f547-40d6-b58a-ed827d7689d0
    evidence:
      source: deep
  scope:
    root: packages/emails
    paths:
      - emails/**
      - src/**
---

# Zod Schema Validation for Template Payloads: Schemas Enforce Constrained Primitives Such Enum

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Schemas SHOULD enforce constrained primitives such as enum definitions and URL validations rather than open-ended string types where domain constraints exist.

### Verify

```bash
# Discover and run the repository type-check script to verify template component props
# Discover and run the repository test suite to validate schema rejection of invalid payloads
```

**Accept when:**
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>