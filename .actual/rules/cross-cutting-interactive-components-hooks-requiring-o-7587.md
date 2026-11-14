---
version: 2
name: cross-cutting-interactive-components-hooks-requiring-o-7587
description: "Adoption of sonner for Toast Notification Management: Interactive Components Hooks Requiring Operation Feedback"
metadata:
  actual:
    adr_id: 75874a90-eab4-4436-823e-dbf1e1f6cd76
    status: accepted
    candidate_group_id: 9abf003d-098e-4232-bd99-6521c772f65f
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/ui/**
      - src/hooks/**
---

# Adoption of sonner for Toast Notification Management: Interactive Components Hooks Requiring Operation Feedback

Scope: applies to `packages/ui/src/components/ui/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for user interface components and client interaction hooks requiring transient status feedback, and shared UI workspace modules managing visual alert presentation.

### Rules

- **R-SON-001** SHOULD: Interactive components and hooks requiring operation feedback SHOULD dispatch alerts through sonner toast functions rather than implementing bespoke notification dialogs or alert overlays.
- **R-SON-002** MANDATORY: Execute the lock-version grounding policy prior to dependency updates and verify toast calls against locked API specifications.
- **R-SON-003** MANDATORY: Enforce client directive boundaries on toast containers and consuming hooks through static analysis checks.

### Verify

```bash
# Discover and run the repository test suite to validate toast notification rendering and interaction hook behavior.
# Discover and run the workspace static analysis and linting checks to ensure compliance with client boundary directives.
```

**Accept when:**
- All toast notification components render without runtime errors under client execution boundaries.
- Interactive hooks successfully dispatch toast notifications using sonner API bindings without regressions.
- Static analysis and test suites pass cleanly across all UI package modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>