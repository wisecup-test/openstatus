---
version: 2
name: cross-cutting-email-templates-not-embed-raw-b924
description: "Adoption of react-email for Transactional Email Composition: Email Templates Not Embed Raw String"
metadata:
  actual:
    adr_id: b9244576-d370-4248-b686-56426e497971
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

# Adoption of react-email for Transactional Email Composition: Email Templates Not Embed Raw String

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for authoring, structuring, and maintaining transactional email templates, and rendering component trees into email markup for outbound notification services.

### Rules

- **R-ACT-001** MUST_NOT: Email templates MUST NOT embed raw string-concatenated HTML or arbitrary unvalidated external styling markup.

### Verify

```bash
# Discover and run type checking, tests, and linter from the dependency manifest
```

**Accept when:**
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines run automated type verification, linting, and template test suites.
</enforcement>