---
version: 2
name: cross-cutting-engineers-inspect-repository-lock-artifa-9ec8
description: "Adoption of react-email for Transactional Email Composition: Engineers Inspect Repository Lock Artifact Determine"
metadata:
  actual:
    adr_id: 9ec8044f-76c3-4046-9eab-5517cf5761c8
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

# Adoption of react-email for Transactional Email Composition: Engineers Inspect Repository Lock Artifact Determine

Scope: applies to `packages/emails/emails/**`, `packages/emails/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ACT-001** MUST: Engineers MUST inspect the repository lock artifact to determine the exact resolved version of react-email before implementing or modifying email templates.

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