---
version: 2
name: cross-cutting-each-icon-module-isolated-permit-08aa
description: "React Adoption for Icon Component Rendering: Each Icon Module Isolated Permit Tree"
metadata:
  actual:
    adr_id: 08aabcaa-dc9a-4f99-92fa-505678a0a45e
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Each Icon Module Isolated Permit Tree

Scope: applies to `packages/icons/src/nucleo/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** SHOULD: Each icon module SHOULD be isolated to permit tree-shaking and granular consumption across client applications.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>