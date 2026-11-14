---
version: 2
name: cross-cutting-new-utility-functions-supporting-status-b049
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: New Utility Functions Supporting Status Presentation"
metadata:
  actual:
    adr_id: b0493769-48ec-42d9-add5-98c34f5eb849
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: New Utility Functions Supporting Status Presentation

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status visualization and event presentation components requiring status formatting, calculation, or shared transformation helpers, and UI block components rendering status indicators, events, or metric timelines.

### Rules

- **R-STATUS-001** MAY: New utility functions supporting status presentation MAY be added to @openstatus/ui/components/blocks/status.utils provided they remain pure and decoupled from direct DOM manipulation.

### Verify

```bash
# Discover and execute the project static analysis, linting, type-checking, and test suite commands from the root manifest
# Example workflow:
# npm run lint
# npm run typecheck
# npm test
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code reviews, and test suites enforce compliance.
</enforcement>