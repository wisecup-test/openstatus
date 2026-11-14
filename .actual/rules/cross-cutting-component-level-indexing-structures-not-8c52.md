---
version: 2
name: cross-cutting-component-level-indexing-structures-not-8c52
description: "Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Component Level Indexing Structures Not Used"
metadata:
  actual:
    adr_id: 8c52201f-aed2-4f98-8e99-aa3bc6206e11
    status: accepted
    candidate_group_id: 450fc1ba-8745-4bc8-87c9-0ea4fbb7ff7d
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Component Level Indexing Structures Not Used

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all client-side UI components handling keyed event and marker collections for presentation rendering within `@openstatus/ui` components.

### Rules

- **R-UI-MAP-001** MUST_NOT: Component-level indexing structures MUST NOT be used as a substitute for persistent datastore infrastructure or shared cross-service caching layers.

### Verify

```bash
# Discover and execute the project test runner script for component unit tests covering status calendar rendering
npm test
# Run typecheck and linting commands to validate component hook signatures and memory indexing patterns
npm run typecheck
npm run lint
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>