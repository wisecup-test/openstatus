---
version: 2
name: cross-cutting-when-consuming-any-external-versioned-93b5
description: "Internal Date Utility isSameDay for Blacklist Date Access: When Consuming Any External Versioned Dependency"
metadata:
  actual:
    adr_id: 93b5fc3c-a2f6-4cb7-9bee-2763b65f439b
    status: accepted
    candidate_group_id: ad6d1ea8-3894-4c41-895c-e77b7786a974
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Internal Date Utility isSameDay for Blacklist Date Access: When Consuming Any External Versioned Dependency

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for date-keyed data structures requiring calendar day matching within data access modules.

### Rules

- **R-ADR-001** MUST: When consuming any external versioned dependency for date manipulation or data access, developers MUST discover the repository lock artifact and verify the exact resolved version prior to implementation.

### Verify

```bash
# Discover and run the project test harness targeting date access and blacklist verification suites.
# Discover and execute repository linting and static type analysis to verify date utility signatures and access patterns.
```

**Accept when:**
- All date access tests pass successfully demonstrating calendar-accurate day matching.
- Static analysis validates type correctness for date objects and comparison parameters.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>