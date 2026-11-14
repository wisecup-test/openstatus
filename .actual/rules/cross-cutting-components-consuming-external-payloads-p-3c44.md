---
version: 2
name: cross-cutting-components-consuming-external-payloads-p-3c44
description: "Zod Schema Validation for Template Payloads: Components Consuming External Payloads Parse Incoming"
metadata:
  actual:
    adr_id: 3c445628-ac2a-48cc-974e-0578279e7c36
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

# Zod Schema Validation for Template Payloads: Components Consuming External Payloads Parse Incoming

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Components consuming external payloads MUST parse incoming props using the defined schema before referencing input properties in template rendering logic.

### Verify

```bash
# Discover and run the repository type-check script
# Discover and run the repository test suite
```

**Accept when:**
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>