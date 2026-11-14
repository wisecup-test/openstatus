---
version: 2
name: cross-cutting-key-indexing-operations-across-event-fe7a
description: "Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Key Indexing Operations Across Event Collections"
metadata:
  actual:
    adr_id: fe7a2b15-01af-4f3e-937e-4af65f8d408b
    status: accepted
    candidate_group_id: 450fc1ba-8745-4bc8-87c9-0ea4fbb7ff7d
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Key Indexing Operations Across Event Collections

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for client-side UI components handling keyed event and marker collections for presentation rendering.

### Rules

- **R-IDX-001** SHOULD: Key indexing operations across event collections SHOULD be memoized within component render hooks to prevent redundant index construction on non-data state changes.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute component unit tests covering status calendar rendering.
# Discover and run the project typecheck and linting commands to validate component hook signatures and memory indexing patterns.
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline executing repository test and linting validation suites, and peer code review verifying that in-memory indexing remains scoped to component presentation layers.
</enforcement>