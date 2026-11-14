---
version: 2
name: cross-cutting-components-utility-modules-within-status-ce48
description: "Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Components Utility Modules Within Status Presentation"
metadata:
  actual:
    adr_id: ce480c8f-ec07-4570-97d4-eaa35d64e301
    status: accepted
    candidate_group_id: 7d5ec3d5-3585-4cb2-a8f1-1aba87155aee
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - src/components/blocks/**
---

# Adoption of @date-fns/utc for UTC Timestamp and Date Processing: Components Utility Modules Within Status Presentation

Scope: applies to `packages/ui/src/components/blocks/**`.

These rules are ALWAYS ACTIVE for status monitoring blocks, timestamp presentation components, and temporal formatting utility functions, as well as any frontend module calculating duration intervals, incident timestamps, or uptime status metrics.

### Rules

- **R-STATUS-001** MUST_NOT: Components and utility modules within the status presentation layer MUST NOT invoke native uncoordinated local time methods or introduce untracked local timezone offsets for status timestamps.

### Verify

```bash
# Discover the project test execution script from the repository manifest and execute it against status block utilities
# Discover the project static analysis and linting script from the repository manifest and execute it to verify dependency imports and type compliance across UI components
```

**Accept when:**
- All unit tests governing status timestamp utilities and components execute without errors or timezone-dependent assertion failures.
- Static analysis confirms all date formatting and utility functions in status blocks import and utilize @date-fns/utc without unresolved types.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing uncoordinated date instantiation or unapproved date libraries in status blocks are blocked pending refactoring.
</enforcement>