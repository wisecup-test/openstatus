---
version: 2
name: cross-cutting-application-modules-not-implement-hoc-a067
description: "Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Application Modules Not Implement Hoc Duplicated"
metadata:
  actual:
    adr_id: a067db2c-4efe-4320-a3a8-2f41a702ab81
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

# Adoption of @openstatus/header-analysis for CDN and Cache Header Evaluation: Application Modules Not Implement Hoc Duplicated

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** MUST_NOT: Application modules MUST NOT implement ad-hoc or duplicated header extraction, cache state evaluation, or provider detection logic outside the canonical header analysis module.

### Verify

```bash
# Discover and execute the repository dependency audit and type checking verification scripts.
# Run the automated test suite across packages consuming the header analysis module to ensure contract compliance.
```

**Accept when:**
- All modules performing HTTP header inspection or CDN edge detection import and execute logic strictly through the designated header analysis module.
- Static type checks and automated project verification suites validate that response headers and cache metadata conform to the standardized schema contracts without ad-hoc header parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation results in build rejection or mandatory refactoring.
</enforcement>