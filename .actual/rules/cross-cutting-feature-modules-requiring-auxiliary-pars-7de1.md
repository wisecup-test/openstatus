---
version: 2
name: cross-cutting-feature-modules-requiring-auxiliary-pars-7de1
description: "Colocated utils Internal Module Architecture: Feature Modules Requiring Auxiliary Parsing Formatting"
metadata:
  actual:
    adr_id: 7de1fcb5-6f02-4bc9-bdf5-f2ab3a98289a
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

# Colocated utils Internal Module Architecture: Feature Modules Requiring Auxiliary Parsing Formatting

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, and endpoint handlers requiring scoped helper utilities.

### Rules

- **R-COL-001** SHOULD: Feature modules requiring auxiliary parsing, formatting, or mock generation logic SHOULD import directly from the local colocated utils module using relative path imports.

### Verify

```bash
# Discover the workspace static analysis and boundary verification script in the repository configuration and execute it
# Discover the repository test runner script and execute domain unit tests covering colocated utility modules
```

**Accept when:**
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>