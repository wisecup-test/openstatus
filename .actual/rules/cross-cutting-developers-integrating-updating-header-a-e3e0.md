---
version: 2
name: cross-cutting-developers-integrating-updating-header-a-e3e0
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Developers Integrating Updating Header Analysis Dependency"
metadata:
  actual:
    adr_id: e3e05c7f-da35-4f23-9e51-9a0ce4769d11
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

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Developers Integrating Updating Header Analysis Dependency

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-01-001** MUST: Developers integrating or updating the header analysis dependency MUST discover the repository resolution lock artifact and verify that the exact locked version satisfies compatibility requirements before modifying code.

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
Claude Code MUST NOT skip or defer verification. All diagnostic network probes and UI summary components must successfully import and consume @openstatus/header-analysis without type errors.
</enforcement>