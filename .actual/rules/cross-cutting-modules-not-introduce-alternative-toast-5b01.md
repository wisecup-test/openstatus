---
version: 2
name: cross-cutting-modules-not-introduce-alternative-toast-5b01
description: "Adoption of sonner for Toast Notification Management: Modules Not Introduce Alternative Toast Notification"
metadata:
  actual:
    adr_id: 5b0162e5-504f-436b-940f-e01624a2f829
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

# Adoption of sonner for Toast Notification Management: Modules Not Introduce Alternative Toast Notification

Scope: applies to `packages/ui/src/components/ui/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SONNER-001** MUST_NOT: Modules MUST NOT introduce alternative toast notification libraries into the shared UI workspace package.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>