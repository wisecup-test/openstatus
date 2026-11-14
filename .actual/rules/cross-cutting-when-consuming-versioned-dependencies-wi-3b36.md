---
version: 2
name: cross-cutting-when-consuming-versioned-dependencies-wi-3b36
description: "Colocated utils Internal Module Architecture: When Consuming Versioned Dependencies Within Internal"
metadata:
  actual:
    adr_id: 3b36805b-78db-482d-9a3c-fad77c1eeaaf
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

# Colocated utils Internal Module Architecture: When Consuming Versioned Dependencies Within Internal

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, endpoint handlers, and internal domain directories requiring scoped helper utilities.

### Rules

- **R-COL-001** MUST: When consuming versioned dependencies within internal utility modules, the engineering team MUST inspect the repository dependency manifest and authoritative lock artifact to verify the exact resolved version prior to implementation.

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