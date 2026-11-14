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

- **R-01** MUST: Developers integrating or updating the header analysis dependency MUST discover the repository resolution lock artifact and verify that the exact locked version satisfies compatibility requirements before modifying code.

### Verify

```bash
# Discover and execute repository type verification and test suites
# (Commands must be derived from the project repository configuration)
```

**Accept when:**
- All diagnostic network probes and UI summary components successfully import and consume @openstatus/header-analysis without type errors.
- Automated test suites covering probe execution and header classification pass cleanly.
- No ad-hoc header parsing routines or duplicate regex extractors exist outside the designated header analysis module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>