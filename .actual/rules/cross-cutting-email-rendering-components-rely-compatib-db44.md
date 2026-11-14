---
version: 2
name: cross-cutting-email-rendering-components-rely-compatib-db44
description: "Adoption of react-email for Transactional Email Composition: Email Rendering Components Rely Compatible Style"
metadata:
  actual:
    adr_id: db449682-a2ba-4c8f-90ad-e65b4d54ed1b
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

# Adoption of react-email for Transactional Email Composition: Email Rendering Components Rely Compatible Style

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EMAIL-001** MUST: Email rendering components MUST rely on email-compatible style primitives provided or supported by react-email rather than browser-specific layout abstractions.

### Verify

```bash
# Discover and run type checking from dependency manifest
# Discover and run test script to validate template rendering and schema integrity
# Discover and run linter to ensure component styling and structure conform to repository standards
```

**Accept when:**
- All email component templates compile and pass type verification without errors.
- Template rendering tests generate compliant HTML markup satisfying schema validations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>