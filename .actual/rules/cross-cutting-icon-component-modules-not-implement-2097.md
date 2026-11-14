---
version: 2
name: cross-cutting-icon-component-modules-not-implement-2097
description: "React Adoption for Icon Component Rendering: Icon Component Modules Not Implement Independent"
metadata:
  actual:
    adr_id: 20972927-e65c-430a-9a9b-079c6d4b7a9a
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Icon Component Modules Not Implement Independent

Scope: applies to `packages/icons/src/nucleo/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ICON-001** MUST_NOT: Icon component modules MUST NOT implement independent ad-hoc prop type signatures or introduce secondary rendering libraries outside the shared icon contract.

### Verify

```bash
# Discover and execute repository typecheck script across workspace packages
# Discover and execute component test and verification scripts from repository manifest
```

**Accept when:**
- All icon component modules compile cleanly with zero type errors against the resolved framework types
- Each icon component exports a functional interface conforming to the shared icon type definition
- Verification scripts for workspace package compilation pass without warnings

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated workspace typecheck and test verification in continuous integration pipelines and code review enforcing standardized component signatures and shared type usage.
</enforcement>