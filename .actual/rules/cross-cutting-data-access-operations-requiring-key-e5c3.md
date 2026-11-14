---
version: 2
name: cross-cutting-data-access-operations-requiring-key-e5c3
description: "Internal Date Utility isSameDay for Blacklist Date Access: Data Access Operations Requiring Key Enumeration"
metadata:
  actual:
    adr_id: e5c36efe-b240-49c4-b9c3-c9e3fe297310
    status: accepted
    candidate_group_id: ad6d1ea8-3894-4c41-895c-e77b7786a974
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Internal Date Utility isSameDay for Blacklist Date Access: Data Access Operations Requiring Key Enumeration

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for date-keyed data structures requiring calendar day matching within data access modules.

### Rules

- **R-DAT-001** SHOULD: Data access operations requiring key enumeration evaluate data collection size and consider normalized key indexing if linear key scans degrade query latency.

### Verify

```bash
# Discover and run the project test harness targeting date access and blacklist verification suites.
# Discover and execute repository linting and static type analysis to verify date utility signatures and access patterns.
```

**Accept when:**
- All date access tests pass successfully demonstrating calendar-accurate day matching.
- Static analysis validates type correctness for date objects and comparison parameters.

<enforcement>
Claude Code MUST NOT skip or defer verification. All date-indexed data structures and lookup mechanisms must use the standardized comparison utility or an approved normalized indexing scheme.
</enforcement>