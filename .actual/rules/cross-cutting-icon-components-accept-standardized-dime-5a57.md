---
version: 2
name: cross-cutting-icon-components-accept-standardized-dime-5a57
description: "React Adoption for Icon Component Rendering: Icon Components Accept Standardized Dimension Class"
metadata:
  actual:
    adr_id: 5a57d266-740d-4bef-87a7-339add8dd7eb
    status: accepted
    candidate_group_id: aadb1909-2891-4b55-8328-f12b33863653
    evidence:
      source: deep
  scope:
    root: packages/icons
    paths:
      - src/nucleo/**
---

# React Adoption for Icon Component Rendering: Icon Components Accept Standardized Dimension Class

Scope: applies to `packages/icons/src/nucleo/**`.

These rules are ALWAYS ACTIVE for all icon component modules and shared type definitions within the iconography package.

### Rules

- **R-ICA-001** SHOULD: Icon components SHOULD accept standardized dimension, class name, and color attributes delegated directly to root vector elements.
- **R-ICA-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the repository.
- **R-ICA-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-ICA-004** MANDATORY: Ensure all new icon components follow functional component patterns returning semantic vector markup conforming to shared types.
- **R-ICA-005** MANDATORY: Verify that export configurations preserve modularity to facilitate tree-shaking during downstream application bundling.

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
Claude Code MUST NOT skip or defer verification. Automated workspace typecheck and test verification in continuous integration pipelines, as well as code review enforcing standardized component signatures and shared type usage, are mandatory.
</enforcement>