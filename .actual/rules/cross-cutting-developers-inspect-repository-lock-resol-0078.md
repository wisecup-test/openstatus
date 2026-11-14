---
version: 2
name: cross-cutting-developers-inspect-repository-lock-resol-0078
description: "React Adoption for Icon Component Rendering: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 00780db8-1c21-4ade-85a9-3e069b45b9d2
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Developers Inspect Repository Lock Resolution Artifact

Scope: applies to `packages/icons/src/nucleo/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ACT-001** MUST: Developers MUST inspect the repository lock or resolution artifact to determine the exact resolved React framework version and confirm API compatibility against official documentation before modifying icon component definitions.
- **R-ACT-002** MUST: Follow functional component patterns returning semantic vector markup conforming to shared types for all new icon components.
- **R-ACT-003** MUST: Verify that export configurations preserve modularity to facilitate tree-shaking during downstream application bundling.

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