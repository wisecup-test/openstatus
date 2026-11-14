---
version: 2
name: cross-cutting-temporal-conversion-logic-encapsulated-w-ed5a
description: "Adoption of @date-fns/utc for UTC Date and Timestamp Management: Temporal Conversion Logic Encapsulated Within Dedicated"
metadata:
  actual:
    adr_id: ed5aae4f-2be9-4c07-bf9e-ebb50ae609d1
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

# Adoption of @date-fns/utc for UTC Date and Timestamp Management: Temporal Conversion Logic Encapsulated Within Dedicated

Scope: applies to `apps/status-page/src/components/content/**`, `apps/status-page/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTC-001** SHOULD: Temporal conversion logic SHOULD be encapsulated within dedicated formatting modules rather than duplicated across individual interface components.

### Verify

```bash
# Discover and execute the repository type-checking, linting, and automated test suites
# (Commands must be discovered from the repository manifest and build configuration)
```

**Accept when:**
- Static analysis confirms all date formatting and timestamp manipulation logic in target modules imports @date-fns/utc.
- Automated test suites pass without timezone-dependent calculation failures or localized rendering variances.
- Dependency audits verify that @date-fns/utc is resolved through the project lock artifact and matches declared manifest ranges.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>