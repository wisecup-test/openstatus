---
version: 2
name: cross-cutting-components-utility-routines-not-perform-4505
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Components Utility Routines Not Perform Custom"
metadata:
  actual:
    adr_id: 4505d476-a704-433e-b945-537966f87810
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

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Components Utility Routines Not Perform Custom

Scope: applies to `apps/status-page/src/components/content/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTC-001** MUST_NOT: Components and utility routines MUST NOT perform custom arithmetic on local timezone offsets when computing UTC status timestamps.

### Verify

```bash
# Discover and execute the repository type-checking and linting suites to ensure date manipulation modules comply with @date-fns/utc typing and import constraints.
# Discover and execute the repository automated test suites covering date formatting helpers and temporal component rendering.
```

**Accept when:**
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>