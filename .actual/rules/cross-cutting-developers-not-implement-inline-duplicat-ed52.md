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

These rules are ALWAYS ACTIVE for all status visualization and event presentation components requiring status formatting, calculation, or shared transformation helpers, as well as UI block components rendering status indicators, events, or metric timelines.

### Rules

- **R-STATUS-001** MUST_NOT: Developers MUST NOT implement inline or duplicate status calculation or formatting functions within individual status block components.

### Verify

```bash
# Discover and execute the project static analysis, type checking, and testing commands from the root manifest
# Example discovery loop:
# 1. Find dependency manifest and build tool
# 2. Run static analysis / linting to validate module import paths and boundary rules
# 3. Run type checker to verify interface compatibility
# 4. Run test suite to validate shared status utility calculations across consuming components
npm run lint
npm run typecheck
npm test
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks, peer code reviews, and static analysis.
</enforcement>