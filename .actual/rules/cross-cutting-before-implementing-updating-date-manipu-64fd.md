---
version: 2
name: cross-cutting-before-implementing-updating-date-manipu-64fd
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Before Implementing Updating Date Manipulation Logic"
metadata:
  actual:
    adr_id: 64fd30bd-7aed-43d7-a1e3-cc188798ed45
    status: accepted
    candidate_group_id: b78de858-5cfd-4774-802a-83e1d237e3bf
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/components/content/**
      - src/lib/**
---

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Before Implementing Updating Date Manipulation Logic

Scope: applies to `apps/status-page/src/components/content/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility modules responsible for parsing, converting, and formatting temporal data, and user interface components that render status timestamps and interactive temporal hover cards.

### Rules

- **R-UTC-001** MUST: Before implementing or updating date manipulation logic, the consumer MUST inspect the repository lock artifact to determine the exact resolved version of @date-fns/utc and verify API availability against that version reference.

### Verify

```bash
# Discover and execute repository type-checking, linting, and automated test suites covering date formatting helpers and temporal component rendering.
# (Exact build and test commands must be discovered dynamically from the repository configuration).
```

**Accept when:**
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and pull request code reviews enforce that all new timestamp operations adopt the standardized library and avoid platform-dependent date arithmetic.
</enforcement>