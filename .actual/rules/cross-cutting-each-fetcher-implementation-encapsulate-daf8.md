---
version: 2
name: cross-cutting-each-fetcher-implementation-encapsulate-daf8
description: "Data Access Dispatch via the Internal fetchers Module: Each Fetcher Implementation Encapsulate Source Specific"
metadata:
  actual:
    adr_id: daf8f4a7-3cdb-4c7b-a880-b4b4030109d5
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Each Fetcher Implementation Encapsulate Source Specific

Scope: applies to `packages/status-fetcher/scripts/**`.

These rules are ALWAYS ACTIVE for data retrieval operations dispatched across heterogeneous entries within the status fetching subsystem.

### Rules

- **R-FTCH-001** MUST: Each fetcher implementation MUST encapsulate source-specific access semantics behind the standard fetcher contract and expose a canHandle compatibility check.
- **R-FTCH-002** MUST: Implement canHandle as a pure predicate function without side effects or external network calls.
- **R-FTCH-003** MUST: Register all available fetcher implementations in an ordered collection before executing entry resolution routines.

### Verify

```bash
# Discover and run the project test suite using the active environment test runner to verify fetcher dispatch and canHandle behavior.
# Discover and execute the repository static type verification script to ensure fetcher interface compliance.
```

**Accept when:**
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration test passes validating fetcher resolution and canHandle execution, and peer code review for all new fetcher additions to verify predicate specificity.
</enforcement>