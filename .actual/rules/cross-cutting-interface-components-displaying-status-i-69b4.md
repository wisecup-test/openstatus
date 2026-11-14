---
version: 2
name: cross-cutting-interface-components-displaying-status-i-69b4
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Interface Components Displaying Status Incident Timelines"
metadata:
  actual:
    adr_id: 69b409da-265e-47f4-83d4-17864b1642d0
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

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Interface Components Displaying Status Incident Timelines

Scope: applies to `apps/status-page/src/components/content/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for utility modules responsible for parsing, converting, and formatting temporal data, and user interface components that render status timestamps and interactive temporal hover cards.

### Rules

- **R-DATE-001** MAY: Interface components displaying status incident timelines MAY format normalized UTC timestamps into localized strings only at the final presentation boundary when user localization is required.

### Verify

```bash
# Discover and execute the repository type-checking and linting suites
# Discover and execute the repository automated test suites covering date formatting helpers and temporal component rendering
```

**Accept when:**
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis checks, pull request code reviews, and dependency audits.
</enforcement>