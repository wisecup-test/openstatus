---
version: 2
name: cross-cutting-date-utility-functions-timestamp-display-8186
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Date Utility Functions Timestamp Display Components"
metadata:
  actual:
    adr_id: 81860bbb-219a-48bd-9a0f-04d98dbe7e81
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Date Utility Functions Timestamp Display Components

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for all status monitoring blocks, timestamp presentation components, and temporal formatting utility functions matching the configured scope.

### Rules

- **R-UTC-001** MUST: All date utility functions and timestamp display components handling status event timestamps MUST use @date-fns/utc for UTC-normalized temporal operations and date parsing.
- **R-UTC-002** MUST: Construct status timestamp dates using UTC wrappers provided by @date-fns/utc before passing them into formatting utilities or UI presentation hooks.
- **R-UTC-003** MUST: Ensure all shared formatting utility functions expose UTC-normalized interfaces to downstream status block components.

### Verify

```bash
# Discover and execute project test execution script from repository manifest against status block utilities
# Discover and execute project static analysis and linting script from repository manifest to verify imports and type compliance
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing uncoordinated date instantiation or unapproved date libraries in status blocks are blocked pending refactoring.
</enforcement>