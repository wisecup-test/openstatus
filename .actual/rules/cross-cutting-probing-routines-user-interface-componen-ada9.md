---
version: 2
name: cross-cutting-probing-routines-user-interface-componen-ada9
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Probing Routines User Interface Components Not"
metadata:
  actual:
    adr_id: ada90a82-ad74-442b-a0f8-3b60e1fdce0c
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

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Probing Routines User Interface Components Not

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** MUST_NOT: Probing routines and user interface components MUST NOT implement custom or ad-hoc header parsing logic that duplicates capabilities provided by @openstatus/header-analysis.

### Verify

```bash
# Discover and execute the repository type verification script to ensure all consumers of the header analysis module conform to exported type signatures.
# Discover and run the project test suite covering network probe execution and diagnostic summary card rendering.
# Inspect module dependency declarations across the repository to verify that header inspection dependencies resolve consistently.
```

**Accept when:**
- All diagnostic network probes and UI summary components successfully import and consume @openstatus/header-analysis without type errors.
- Automated test suites covering probe execution and header classification pass cleanly.
- No ad-hoc header parsing routines or duplicate regex extractors exist outside the designated header analysis module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>