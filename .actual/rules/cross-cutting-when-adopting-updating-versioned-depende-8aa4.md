---
version: 2
name: cross-cutting-when-adopting-updating-versioned-depende-8aa4
description: "Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: When Adopting Updating Versioned Dependencies Supporting"
metadata:
  actual:
    adr_id: 8aa4ec45-8e2c-4ae3-8516-9d517c8d5946
    status: accepted
    candidate_group_id: 450fc1ba-8745-4bc8-87c9-0ea4fbb7ff7d
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: When Adopting Updating Versioned Dependencies Supporting

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for client-side UI components handling keyed event and marker collections for presentation rendering.

### Rules

- **R-MAP-001** MUST: When adopting or updating versioned dependencies supporting UI components, developers MUST inspect the repository dependency manifest and lock artifact to resolve the exact installed dependency version before invoking component APIs.
- **R-MAP-002** MUST: Execute the lock-version grounding process (find manifest, identify build tool, inspect lock artifact for exact resolved version, fetch documentation for that exact version, confirm APIs exist, and re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-MAP-003** MUST: Construct index maps by day key or unique identifier immediately prior to rendering iteration, ensuring that composite keys maintain deterministic formatting.
- **R-MAP-004** SHOULD: Encapsulate Map index generation within memoization hooks tied strictly to collection reference identity to avoid memory pressure from large event collections.
- **R-MAP-005** MUST NOT: Mutate index structures outside hook boundaries, and MUST NOT allow component indices to leak to global scope.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute component unit tests covering status calendar rendering.
# Discover and run the project typecheck and linting commands to validate component hook signatures and memory indexing patterns.
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and peer code reviews enforce compliance by blocking unindexed linear scans in high-frequency render loops, leaking component indices to global scope, or bypassing persistent data storage architecture.
</enforcement>