---
version: 2
name: cross-cutting-modules-extend-shared-header-analysis-e579
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Modules Extend Shared Header Analysis Configurations"
metadata:
  actual:
    adr_id: e5796c6b-0103-445b-9943-15c1fe4dae28
    status: accepted
    candidate_group_id: 4dc8ceff-793e-4bd6-86a1-3c4c1bee59c4
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/cdn-checker/**
---

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Modules Extend Shared Header Analysis Configurations

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** MAY: Modules MAY extend shared header analysis configurations when accommodating non-standard diagnostic headers, provided updates are integrated into the shared library interface.
- **R-HDR-002** MANDATORY: Consume shared header analysis routines rather than manipulating raw response header maps directly within presentation or probing modules.
- **R-HDR-003** MANDATORY: Couple header analysis outputs with schema validation schemas to enforce runtime data contracts.

### Verify

```bash
# Discover and execute the repository dependency audit and type checking verification scripts.
# Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.
npm test
npm run typecheck
```

**Accept when:**
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>