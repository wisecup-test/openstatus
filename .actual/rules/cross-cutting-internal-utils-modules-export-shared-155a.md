---
version: 2
name: cross-cutting-internal-utils-modules-export-shared-155a
description: "Colocated utils Internal Module Architecture: Internal Utils Modules Export Shared Constants"
metadata:
  actual:
    adr_id: 155af032-d59b-492a-ab95-d39ef8cac42b
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

# Colocated utils Internal Module Architecture: Internal Utils Modules Export Shared Constants

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, endpoint handlers, and internal domain directories managing localized data transformation, parsing, and formatting routines.

### Rules

- **R-UTIL-001** MAY: Internal utils modules MAY re-export shared constants or validated schema references required for localized domain transformations.
- **R-UTIL-002** MUST: Colocate helper functions in an internal utils module within the same directory as the consumers, keeping functions pure and focused on domain-specific transformations.
- **R-UTIL-003** MUST: Ensure that exports from colocated utils modules remain private to the enclosing feature directory unless explicitly approved for promotion to shared workspace scope.

### Verify

```bash
# Discover and execute the workspace static analysis and boundary verification script
# Discover and execute the repository test runner script covering colocated utility modules
```

**Accept when:**
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>