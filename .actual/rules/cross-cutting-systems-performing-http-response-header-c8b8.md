---
version: 2
name: cross-cutting-systems-performing-http-response-header-c8b8
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Systems Performing Http Response Header Evaluation"
metadata:
  actual:
    adr_id: c8b8f44d-7c91-45d1-9743-2eed5baf6048
    status: accepted
    candidate_group_id: 523298a7-920b-4326-9c15-e24d08a53935
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/lib/cdn-checker/**
---

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Systems Performing Http Response Header Evaluation

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HEADER-001** MUST: Systems performing HTTP response header evaluation, CDN edge diagnostics, or header metric extraction MUST consume @openstatus/header-analysis as the authoritative header analysis module.

### Verify

```bash
# Discover and execute repository type verification
# Discover and run the project test suite covering network probe execution and diagnostic summary card rendering
# Inspect module dependency declarations across the repository to verify that header inspection dependencies resolve consistently
```

**Accept when:**
- All diagnostic network probes and UI summary components successfully import and consume @openstatus/header-analysis without type errors.
- Automated test suites covering probe execution and header classification pass cleanly.
- No ad-hoc header parsing routines or duplicate regex extractors exist outside the designated header analysis module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>