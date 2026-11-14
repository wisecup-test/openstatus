---
version: 2
name: cross-cutting-modules-not-import-internal-domain-9e90
description: "Colocated utils Internal Module Architecture: Modules Not Import Internal Domain Specific"
metadata:
  actual:
    adr_id: 9e90a1d3-6e3e-4fac-8060-5f7b3ed4ac79
    status: accepted
    candidate_group_id: 6a719b3d-fbf7-45b8-8a1e-ad0d8248fee7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
      - src/content/**
      - src/lib/checker/**
---

# Colocated utils Internal Module Architecture: Modules Not Import Internal Domain Specific

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, endpoint handlers requiring scoped helper utilities, and internal domain directories managing localized data transformation, parsing, and formatting routines.

### Rules

- **R-DOMAIN-001** MUST_NOT: Modules MUST NOT import internal domain-specific helpers from adjacent feature sibling directories when those helpers belong to a distinct colocated utils module.

### Verify

```bash
# Discover and execute the workspace static analysis and boundary verification script
# Discover and execute the repository test runner script for domain unit tests
```

**Accept when:**
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>