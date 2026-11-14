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

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute component unit tests covering status calendar rendering.
# Discover and run the project typecheck and linting commands to validate component hook signatures and memory indexing patterns.
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>