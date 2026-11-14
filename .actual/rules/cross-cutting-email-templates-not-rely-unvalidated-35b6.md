---
version: 2
name: cross-cutting-email-templates-not-rely-unvalidated-35b6
description: "Zod Schema Validation for Template Payloads: Email Templates Not Rely Unvalidated Typescript"
metadata:
  actual:
    adr_id: 35b6cea2-687c-4e41-823e-eb6c71b03220
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

# Zod Schema Validation for Template Payloads: Email Templates Not Rely Unvalidated Typescript

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Email templates MUST NOT rely on unvalidated TypeScript interfaces alone for runtime boundaries where external or untrusted data is ingested.

### Verify

```bash
# Discover and run the repository type-check script to verify that all template component props align with schema definitions.
# Discover and run the repository test suite to validate that schemas reject invalid payload structures.
```

**Accept when:**
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration type checking, automated test pipelines, and peer review on pull requests touching template components and schema definitions.
</enforcement>