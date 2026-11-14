---
version: 2
name: cross-cutting-fetcher-dispatchers-handle-lookup-misses-9794
description: "Data Access Dispatch via the Internal fetchers Module: Fetcher Dispatchers Handle Lookup Misses Explicitly"
metadata:
  actual:
    adr_id: 9794e15a-a986-47e8-bb83-97c7319edb27
    status: accepted
    candidate_group_id: ab4bb405-8fa9-4272-8393-c81e5691d4c3
    evidence:
      source: deep
  scope:
    root: packages/status-fetcher
    paths:
      - scripts/**
---

# Data Access Dispatch via the Internal fetchers Module: Fetcher Dispatchers Handle Lookup Misses Explicitly

Scope: applies to `packages/status-fetcher/scripts/**`.

These rules are ALWAYS ACTIVE for data retrieval operations dispatched across heterogeneous entries within the status fetching subsystem.

### Rules

- **R-FETCH-001** SHOULD: Fetcher dispatchers SHOULD handle lookup misses explicitly when no registered handler satisfies the canHandle predicate for a given entry.
- **R-FETCH-002** MANDATORY: Register all available fetcher implementations in an ordered collection before executing entry resolution routines.
- **R-FETCH-003** MANDATORY: Implement canHandle as a pure predicate function without side effects or external network calls.
- **R-FETCH-004** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library (find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).

### Verify

```bash
# Discover and run the project test suite using the active environment test runner
# Discover and execute the repository static type verification script
```

**Accept when:**
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration test passes, peer code review, and static type checks. Pull requests bypassing canHandle dispatch in favor of hardcoded concrete fetcher calls must be blocked.
</enforcement>