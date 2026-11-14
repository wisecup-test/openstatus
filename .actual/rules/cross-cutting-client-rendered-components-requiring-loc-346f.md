---
version: 2
name: cross-cutting-client-rendered-components-requiring-loc-346f
description: "date-fns Adoption for Modular Date Formatting and Localization: Client Rendered Components Requiring Localized Date"
metadata:
  actual:
    adr_id: 346f8802-6c0c-4439-a415-d219f9374bbe
    status: accepted
    candidate_group_id: 96fcb868-41ab-47cd-9c75-3a2bf49f4c7d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/**
      - src/lib/**
---

# date-fns Adoption for Modular Date Formatting and Localization: Client Rendered Components Requiring Localized Date

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility formatting modules handling timestamp and date transformations, and client-side user interface components rendering temporal data or localized timestamps.

### Rules

- **R-DATE-001** MUST: Client-rendered components requiring localized date presentations MUST consume locale definitions through the established client-side date provider context.

### Verify

```bash
# Discover and execute project test execution script for date formatting utilities
npm test
# Run static analysis and linting script
npm run lint
# Run type verification script across client provider components
npm run typecheck
```

**Accept when:**
- All date formatting utility test suites execute successfully with consistent UTC and localized outputs.
- Static analysis reports zero violations for unapproved date manipulation libraries or direct mutations.
- Type checking passes without errors across all formatting modules and provider components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>