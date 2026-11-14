---
version: 2
name: cross-cutting-consumers-not-perform-direct-string-fb56
description: "Internal Date Utility isSameDay for Blacklist Date Access: Consumers Not Perform Direct String Equality"
metadata:
  actual:
    adr_id: fb56c04c-8732-4cbb-b304-e0e14b84ebf6
    status: accepted
    candidate_group_id: ad6d1ea8-3894-4c41-895c-e77b7786a974
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Internal Date Utility isSameDay for Blacklist Date Access: Consumers Not Perform Direct String Equality

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DATE-001** MUST_NOT: Consumers MUST NOT perform direct string equality checks when matching date keys against runtime day values.

### Verify

```bash
# Discover and run the project test harness targeting date access and blacklist verification suites.
# Discover and execute repository linting and static type analysis to verify date utility signatures and access patterns.
```

**Accept when:**
- All date access tests pass successfully demonstrating calendar-accurate day matching.
- Static analysis validates type correctness for date objects and comparison parameters.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>