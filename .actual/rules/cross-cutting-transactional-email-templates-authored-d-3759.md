---
version: 2
name: cross-cutting-transactional-email-templates-authored-d-3759
description: "Adoption of react-email for Transactional Email Composition: Transactional Email Templates Authored Declarative Component"
metadata:
  actual:
    adr_id: 37592844-1835-48c4-bdd8-264d05dfd2b1
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

# Adoption of react-email for Transactional Email Composition: Transactional Email Templates Authored Declarative Component

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRAN-001** MUST: All transactional email templates MUST be authored as declarative component hierarchies using react-email primitives to generate email-safe markup.

### Verify

```bash
# Discover and run the project type checking script from the dependency manifest.
# Discover and run the project test script to validate template rendering and schema integrity.
# Discover and run the project linter to ensure component styling and structure conform to repository standards.
```

**Accept when:**
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory and enforced by continuous integration pipelines and automated checks.
</enforcement>