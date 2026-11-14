---
version: 2
name: cross-cutting-developers-not-implement-inline-duplicat-ed52
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Not Implement Inline Duplicate Status"
metadata:
  actual:
    adr_id: ed52aec2-430e-40a9-bad2-50df9dbe0f6d
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: Developers Not Implement Inline Duplicate Status

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status visualization, event presentation, and UI block components requiring status formatting, calculation, or shared transformation helpers.

### Rules

- **R-STATUS-001** MUST_NOT: Developers MUST NOT implement inline or duplicate status calculation or formatting functions within individual status block components.

### Verify

```bash
# Discover and execute the project static analysis, type checking, and test commands from the root manifest
# Example discovery sequence:
# 1. Inspect package.json for lint, typecheck, and test scripts
# 2. Run lint/static analysis
npm run lint

# Run type checker
npm run typecheck

# Run test suite
npm test
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration and peer code reviews strictly enforce that status logic is imported from the canonical utility module rather than reimplemented locally.
</enforcement>