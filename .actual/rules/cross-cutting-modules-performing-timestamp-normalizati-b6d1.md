---
version: 2
name: cross-cutting-modules-performing-timestamp-normalizati-b6d1
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Modules Performing Timestamp Normalization Date Formatting"
metadata:
  actual:
    adr_id: b6d1bf6e-4e92-48d3-b0b3-4d1e8a60eada
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

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Modules Performing Timestamp Normalization Date Formatting

Scope: applies to `apps/status-page/src/components/content/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for modules performing timestamp normalization, date formatting, and interactive temporal rendering.

### Rules

- **R-UTC-001** MUST: Modules performing timestamp normalization, date formatting, and interactive temporal rendering MUST adopt @date-fns/utc for all UTC date handling operations.

### Verify

```bash
# Discover and execute repository type-checking, linting, and automated test suites covering date formatting helpers and temporal components
```

**Accept when:**
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>