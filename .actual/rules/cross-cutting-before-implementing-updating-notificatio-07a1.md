---
version: 2
name: cross-cutting-before-implementing-updating-notificatio-07a1
description: "Adoption of sonner for Toast Notification Management: Before Implementing Updating Notification Components That"
metadata:
  actual:
    adr_id: 07a11ca6-47e1-4721-bead-836951f33077
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

# Adoption of sonner for Toast Notification Management: Before Implementing Updating Notification Components That

Scope: applies to `packages/ui/src/components/ui/**`, `packages/ui/src/hooks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SONNER-001** MUST: Before implementing or updating notification components that consume sonner, developers MUST inspect the repository lock artifact to determine the exact resolved dependency version and consult documentation matching that exact version.

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