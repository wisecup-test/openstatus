---
version: 2
name: cross-cutting-components-rendering-status-representati-8020
description: "Adoption of @openstatus/ui/components/blocks/status.utils Module: Components Rendering Status Representations Import Shared"
metadata:
  actual:
    adr_id: 80205248-c28e-4e76-b7ea-359f19652f3f
    status: accepted
    candidate_group_id: 459b23c3-ed82-460d-a6fd-c36d98a0ae9e
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @openstatus/ui/components/blocks/status.utils Module: Components Rendering Status Representations Import Shared

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-STATUS-001** MUST: Components rendering status representations MUST import shared calculation, formatting, and helper logic exclusively from @openstatus/ui/components/blocks/status.utils.

### Verify

```bash
# Discover and execute the project static analysis, linting, type-checking, and test suite commands from the project manifest.
```

**Accept when:**
- Static analysis and linting pass with zero import boundary or module resolution errors across status block components.
- All unit and integration tests for status utilities and consuming status components pass without failure.
- The type checker completes with zero errors confirming strict interface compliance across utility call sites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>