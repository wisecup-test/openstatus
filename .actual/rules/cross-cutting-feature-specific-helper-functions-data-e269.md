---
version: 2
name: cross-cutting-feature-specific-helper-functions-data-e269
description: "Colocated utils Internal Module Architecture: Feature Specific Helper Functions Data Transformations"
metadata:
  actual:
    adr_id: e2692d6e-c4e0-4b1f-9341-57c3efe1b62c
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

# Colocated utils Internal Module Architecture: Feature Specific Helper Functions Data Transformations

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, endpoint handlers, and internal domain directories managing localized data transformation, parsing, and formatting routines.

### Rules

- **R-UTIL-001** MUST: Feature-specific helper functions, data transformations, and local abstractions MUST be encapsulated in colocated utils modules within the consuming feature domain directory rather than defined in global utility modules.
- **R-UTIL-002** MUST: Maintain helper functions as pure and focused on domain-specific transformations within the same directory as the consumers.
- **R-UTIL-003** MUST: Keep exports from colocated utils modules private to the enclosing feature directory unless explicitly approved for promotion to shared workspace scope.

### Verify

```bash
# Discover and execute the workspace static analysis and boundary verification script
# Discover and execute the repository test runner script for domain unit tests covering colocated utility modules
```

**Accept when:**
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated structural import boundary checks and peer code reviews enforce compliance.
</enforcement>