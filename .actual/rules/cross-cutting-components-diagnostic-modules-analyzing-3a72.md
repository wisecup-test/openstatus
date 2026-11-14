---
version: 2
name: cross-cutting-components-diagnostic-modules-analyzing-3a72
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Components Diagnostic Modules Analyzing Http Edge"
metadata:
  actual:
    adr_id: 3a7274a9-6ad6-4ccd-8951-b54c3a45d12f
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

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Components Diagnostic Modules Analyzing Http Edge

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** MUST: Components and diagnostic modules analyzing HTTP edge response headers, cache status, or CDN provider signatures MUST consume the @openstatus/header-analysis module as the single source of truth for header evaluation.

### Verify

```bash
# Discover and execute the repository dependency audit and type checking verification scripts.
# Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.
```

**Accept when:**
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>