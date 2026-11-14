---
version: 2
name: cross-cutting-date-formatting-operations-requiring-utc-3280
description: "date-fns Adoption for Modular Date Formatting and Localization: Date Formatting Operations Requiring Utc Standardization"
metadata:
  actual:
    adr_id: 3280a5e4-b9d3-4b3f-b687-bcc9f35ea72e
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

# date-fns Adoption for Modular Date Formatting and Localization: Date Formatting Operations Requiring Utc Standardization

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility formatting modules handling timestamp and date transformations, and client-side user interface components rendering temporal data or localized timestamps.

### Rules

- **R-DATE-001** MUST: Date formatting operations requiring UTC standardization MUST normalize timestamps through dedicated UTC utility wrappers before applying presentation formatting.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute unit tests covering date formatting utilities.
# Discover the project static analysis and linting script from the repository manifest and run validation to verify absence of disallowed direct date mutations.
# Discover the project type verification script from the repository manifest and run type checks across client provider components.
```

**Accept when:**
- All date formatting utility test suites execute successfully with consistent UTC and localized outputs.
- Static analysis reports zero violations for unapproved date manipulation libraries or direct mutations.
- Type checking passes without errors across all formatting modules and provider components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>