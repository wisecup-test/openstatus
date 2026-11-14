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

- **R-FETCH-001** MUST: Each fetcher implementation MUST encapsulate source-specific access semantics behind the standard fetcher contract and expose a canHandle compatibility check.
- **R-FETCH-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-FETCH-003** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact version, (4) Look up official docs for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-FETCH-004** MANDATORY: Register all available fetcher implementations in an ordered collection before executing entry resolution routines.
- **R-FETCH-005** MANDATORY: Implement canHandle as a pure predicate function without side effects or external network calls.

### Verify

```bash
# Discover and run the project test suite using the active environment test runner to verify fetcher dispatch and canHandle behavior.
# Discover and execute the repository static type verification script to ensure fetcher interface compliance.
```

**Accept when:**
- All registered fetchers satisfy the common interface and pass test suite assertions.
- Entry dispatch reliably resolves the compatible fetcher without unhandled lookup rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test passes validating fetcher resolution and canHandle execution are required. Pull requests bypassing canHandle dispatch in favor of hardcoded concrete fetcher calls must be blocked.
</enforcement>