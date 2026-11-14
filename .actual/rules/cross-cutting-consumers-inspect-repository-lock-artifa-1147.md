---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-1147
description: "Zod Schema Validation for Template Payloads: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 1147c743-6669-47d7-bc3b-7e400483893a
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

# Zod Schema Validation for Template Payloads: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for transactional email template components receiving dynamic runtime props and runtime configuration modules parsing environment variables.

### Rules

- **R-ZOD-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of the schema validation dependency before implementation.

### Verify

```bash
# Discover and run the repository type-check script to verify that all template component props align with schema definitions.
# Discover and run the repository test suite to validate that schemas reject invalid payload structures.
```

**Accept when:**
- All email template components validate runtime payload props using schema definitions prior to rendering.
- The repository test suite passes with zero type-check or schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checking and automated test pipelines enforce compliance.
</enforcement>