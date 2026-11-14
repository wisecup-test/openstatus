---
version: 2
name: cross-cutting-client-rendered-components-presenting-ti-a879
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Client Rendered Components Presenting Timestamp Hover"
metadata:
  actual:
    adr_id: a879cbf3-849c-484a-b69f-e32fd3c50f62
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Client Rendered Components Presenting Timestamp Hover

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status monitoring blocks, timestamp presentation components, and temporal formatting utility modules calculating duration intervals, incident timestamps, or uptime status metrics.

### Rules

- **R-UTC-001** MUST: Client-rendered components presenting timestamp hover states or status intervals MUST derive display dates through UTC-normalized constructs to prevent hydration divergence between rendering phases.

### Verify

```bash
# Discover and run the project test execution script from the repository manifest against status block utilities
# Discover and run the project static analysis and linting script from the repository manifest
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipelines executing test suites across diverse timezone environments and peer code review verification.
</enforcement>