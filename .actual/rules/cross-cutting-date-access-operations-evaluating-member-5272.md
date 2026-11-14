---
version: 2
name: cross-cutting-date-access-operations-evaluating-member-5272
description: "Internal Date Utility isSameDay for Blacklist Date Access: Date Access Operations Evaluating Membership Against"
metadata:
  actual:
    adr_id: 52726f69-8d80-47d2-9ccd-fcd40570e928
    status: accepted
    candidate_group_id: ad6d1ea8-3894-4c41-895c-e77b7786a974
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Internal Date Utility isSameDay for Blacklist Date Access: Date Access Operations Evaluating Membership Against

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-30-001** MUST: Date access operations evaluating membership against collection keys representing calendar dates MUST use the isSameDay comparison utility rather than raw string or timestamp equality.

### Verify

```bash
# Discover and run the project test harness targeting date access and blacklist verification suites.
# Discover and execute repository linting and static type analysis to verify date utility signatures and access patterns.
```

**Accept when:**
- All date access tests pass successfully demonstrating calendar-accurate day matching.
- Static analysis validates type correctness for date objects and comparison parameters.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated test suites and peer code reviews verify compliance.
</enforcement>