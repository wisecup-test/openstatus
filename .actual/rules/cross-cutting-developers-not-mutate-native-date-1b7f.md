---
version: 2
name: cross-cutting-developers-not-mutate-native-date-1b7f
description: "date-fns Adoption for Modular Date Formatting and Localization: Developers Not Mutate Native Date Instances"
metadata:
  actual:
    adr_id: 1b7fa887-00b7-49dd-81bb-69f77b6a3bc8
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

# date-fns Adoption for Modular Date Formatting and Localization: Developers Not Mutate Native Date Instances

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility formatting modules handling timestamp and date transformations, and client-side user interface components rendering temporal data or localized timestamps.

### Rules

- **R-DATE-001** MUST_NOT: Developers MUST_NOT mutate native date instances directly or instantiate global date formatters outside the standardized date utility modules.

### Verify

```bash
# Discover and execute project test execution script for date formatting utilities
# Discover and run project static analysis and linting script
# Discover and run project type verification script across client provider components
```

**Accept when:**
- All date formatting utility test suites execute successfully with consistent UTC and localized outputs.
- Static analysis reports zero violations for unapproved date manipulation libraries or direct mutations.
- Type checking passes without errors across all formatting modules and provider components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>