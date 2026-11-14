---
version: 2
name: cross-cutting-date-utility-functions-timestamp-display-8186
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Date Utility Functions Timestamp Display Components"
metadata:
  actual:
    adr_id: 81860bbb-219a-48bd-9a0f-04d98dbe7e81
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Date Utility Functions Timestamp Display Components

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTC-001** MUST: All date utility functions and timestamp display components handling status event timestamps MUST use @date-fns/utc for UTC-normalized temporal operations and date parsing.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute it against status block utilities
# Discover the project static analysis and linting script from the repository manifest and execute it to verify dependency imports
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>