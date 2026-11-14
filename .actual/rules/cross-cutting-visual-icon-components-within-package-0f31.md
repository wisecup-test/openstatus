---
version: 2
name: cross-cutting-visual-icon-components-within-package-0f31
description: "React Adoption for Icon Component Rendering: Visual Icon Components Within Package Authored"
metadata:
  actual:
    adr_id: 0f3156de-df02-423d-b9de-eba0765157c1
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Visual Icon Components Within Package Authored

Scope: applies to `packages/icons/src/nucleo/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ACT-001** MUST: All visual icon components within the icon package MUST be authored as React functional components that import React and conform to the standardized shared icon contract types.

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