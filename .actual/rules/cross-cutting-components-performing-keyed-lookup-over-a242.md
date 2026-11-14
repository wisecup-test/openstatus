---
version: 2
name: cross-cutting-components-performing-keyed-lookup-over-a242
description: "Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Components Performing Keyed Lookup Over Client"
metadata:
  actual:
    adr_id: a24249c8-cd34-4338-a7f7-08407cbbff06
    status: accepted
    candidate_group_id: 450fc1ba-8745-4bc8-87c9-0ea4fbb7ff7d
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Map-Based Ephemeral In-Memory Indexing in @openstatus/ui Components: Components Performing Keyed Lookup Over Client

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UI-001** MUST: UI components performing keyed lookup over client-side event datasets MUST use localized Map indexing structures scoped to component lifecycle hooks to ensure constant-time data access.

### Verify

```bash
# Discover the project manifest, build tools, and test/typecheck scripts from the repository.
# Run component unit tests covering status calendar rendering:
# e.g., npm test -- --runInBand (or project equivalent discovered from manifest)
# Run static type analysis and linting:
# e.g., npm run typecheck && npm run lint (or project equivalents discovered from manifest)
```

**Accept when:**
- All component unit tests pass successfully without memory leaks or render cycle regressions.
- Static type analysis and lint checks verify that Map lookups adhere to strict typing and proper memoization hook dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>