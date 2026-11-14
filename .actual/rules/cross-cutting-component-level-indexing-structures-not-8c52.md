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

- **R-INDEX-001** MUST_NOT: Component-level indexing structures MUST NOT be used as a substitute for persistent datastore infrastructure or shared cross-service caching layers.
- **R-INDEX-002** MANDATORY: Construct index maps by day key or unique identifier immediately prior to rendering iteration, ensuring that composite keys maintain deterministic formatting.
- **R-INDEX-003** MANDATORY: Avoid mutating index structures outside hook boundaries to maintain referential predictability across UI component renders.
- **R-INDEX-004** MANDATORY: Encapsulate Map index generation within memoization hooks tied strictly to collection reference identity to prevent memory pressure from large event collections.

### Verify

```bash
# Discover the repository test runner script from the project manifest and execute component unit tests covering status calendar rendering.
# Discover and run the project typecheck and linting commands to validate component hook signatures and memory indexing patterns.
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.
- Component-level indexing remains strictly scoped to presentation layers and transient view models.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and peer review enforce that in-memory indexing remains scoped to component presentation layers without leaking to global scope or bypassing persistent data storage architecture.
</enforcement>