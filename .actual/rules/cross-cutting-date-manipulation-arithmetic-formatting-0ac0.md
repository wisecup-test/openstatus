---
version: 2
name: cross-cutting-date-manipulation-arithmetic-formatting-0ac0
description: "date-fns Adoption for Modular Date Formatting and Localization: Date Manipulation Arithmetic Formatting Standardize Fns"
metadata:
  actual:
    adr_id: 0ac032ed-0396-4c57-b8ef-2734094c0692
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

# date-fns Adoption for Modular Date Formatting and Localization: Date Manipulation Arithmetic Formatting Standardize Fns

Scope: applies to `apps/status-page/src/components/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DATE-001** MUST: All date manipulation, arithmetic, and formatting MUST standardize on date-fns functional utilities rather than custom timestamp calculations or alternative date libraries.

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