---
version: 2
name: cross-cutting-date-utility-functions-import-granular-5f99
description: "date-fns Adoption for Modular Date Formatting and Localization: Date Utility Functions Import Granular Helper"
metadata:
  actual:
    adr_id: 5f996d52-abd6-41e8-b730-aacf4332cfce
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

# date-fns Adoption for Modular Date Formatting and Localization: Date Utility Functions Import Granular Helper

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility formatting modules, client-side user interface components rendering temporal data, and localized timestamps.

### Rules

- **R-DATE-001** SHOULD: Date utility functions SHOULD import granular helper functions directly from date-fns modules rather than aggregating entire namespace trees to preserve dead-code elimination.

### Verify

```bash
# Discover and run the project test execution script covering date formatting utilities
# Discover and run static analysis and linting scripts to verify absence of disallowed direct date mutations
# Discover and run type checks across client provider components
```

**Accept when:**
- All date formatting utility test suites execute successfully with consistent UTC and localized outputs.
- Static analysis reports zero violations for unapproved date manipulation libraries or direct mutations.
- Type checking passes without errors across all formatting modules and provider components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>