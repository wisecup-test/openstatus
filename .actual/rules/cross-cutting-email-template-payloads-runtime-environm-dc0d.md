---
version: 2
name: cross-cutting-email-template-payloads-runtime-environm-dc0d
description: "Zod Schema Validation for Template Payloads: Email Template Payloads Runtime Environment Inputs"
metadata:
  actual:
    adr_id: dc0dd7fa-28cf-4dc8-a1f6-cd71688fcc17
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

# Zod Schema Validation for Template Payloads: Email Template Payloads Runtime Environment Inputs

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for transactional email template components receiving dynamic runtime props and runtime configuration modules parsing environment variables.

### Rules

- **R-ZOD-001** MUST: All email template payloads and runtime environment inputs MUST declare an explicit Zod schema to validate structure and enforce type safety at the ingestion boundary.

### Verify

```bash
# Discover and run the repository type-check script to verify that all template component props align with schema definitions.
# Discover and run the repository test suite to validate that schemas reject invalid payload structures.
```

**Accept when:**
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, automated test pipelines, and peer review enforce these requirements.
</enforcement>