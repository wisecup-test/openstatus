---
version: 2
name: cross-cutting-diagnostic-probe-implementations-combine-74b3
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Diagnostic Probe Implementations Combine Openstatus Header"
metadata:
  actual:
    adr_id: 74b380e6-9b37-4425-98b8-dfa277dcc167
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

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Diagnostic Probe Implementations Combine Openstatus Header

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD: Diagnostic probe implementations SHOULD combine @openstatus/header-analysis with shared checker utility modules to maintain consistent execution boundaries between network transport and header evaluation.

### Verify

```bash
# Discover and execute the repository type verification script
# Discover and run the project test suite covering network probe execution and diagnostic summary card rendering
# Inspect module dependency declarations across the repository to verify that header inspection dependencies resolve consistently
```

**Accept when:**
- All diagnostic network probes and UI summary components successfully import and consume @openstatus/header-analysis without type errors.
- Automated test suites covering probe execution and header classification pass cleanly.
- No ad-hoc header parsing routines or duplicate regex extractors exist outside the designated header analysis module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks and peer code reviews.
</enforcement>