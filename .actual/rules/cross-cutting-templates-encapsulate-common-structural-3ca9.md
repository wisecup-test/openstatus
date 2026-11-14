---
version: 2
name: cross-cutting-templates-encapsulate-common-structural-3ca9
description: "Adoption of react-email for Transactional Email Composition: Templates Encapsulate Common Structural Wrappers Composing"
metadata:
  actual:
    adr_id: 3ca94a30-f8f7-4be5-9b0b-15a210c63865
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

# Adoption of react-email for Transactional Email Composition: Templates Encapsulate Common Structural Wrappers Composing

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRE-001** MUST: Templates MUST encapsulate common structural wrappers by composing shared layout and styling components rather than duplicating container markup.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>