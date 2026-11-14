---
version: 2
name: cross-cutting-network-client-requests-executed-during-17e0
description: "Adoption of @openstatus/header-analysis for HTTP Header Analysis: Network Client Requests Executed During Diagnostic"
metadata:
  actual:
    adr_id: 17e0aeda-9392-4b88-a78d-bd049ecad77b
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

# Adoption of @openstatus/header-analysis for HTTP Header Analysis: Network Client Requests Executed During Diagnostic

Scope: applies to `apps/web/src/app/**`, `apps/web/src/lib/cdn-checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Network client requests executed during diagnostic probing MUST configure explicit request timeouts and no-store cache directives to ensure unbiased diagnostic telemetry.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>