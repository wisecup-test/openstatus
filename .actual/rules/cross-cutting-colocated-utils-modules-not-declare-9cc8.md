---
version: 2
name: cross-cutting-colocated-utils-modules-not-declare-9cc8
description: "Colocated utils Internal Module Architecture: Colocated Utils Modules Not Declare Stateful"
metadata:
  actual:
    adr_id: 9cc89c98-e31a-42d4-9c99-72b7b101cb79
    status: accepted
    candidate_group_id: 6a719b3d-fbf7-45b8-8a1e-ad0d8248fee7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/og/**
      - src/content/**
      - src/lib/checker/**
---

# Colocated utils Internal Module Architecture: Colocated Utils Modules Not Declare Stateful

Scope: applies to `apps/web/src/app/api/og/**`, `apps/web/src/content/**`, `apps/web/src/lib/checker/**`.

These rules are ALWAYS ACTIVE for feature-level modules, content transformation logic, mock providers, endpoint handlers, and internal domain directories managing localized data transformation, parsing, and formatting routines.

### Rules

- **R-UTIL-001** SHOULD_NOT: Colocated utils modules SHOULD NOT declare stateful side-effects or export global singleton lifecycle handlers that span beyond their parent feature directory.

### Verify

```bash
# Discover and execute the workspace static analysis and boundary verification script
# Discover and execute the repository test runner script covering domain unit tests for colocated utility modules
```

**Accept when:**
- Static boundary verification confirms zero disallowed cross-domain imports referencing internal colocated utils modules.
- Unit test suites associated with feature domains and colocated utility routines execute with complete pass results.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated structural import boundary checks and peer code review verification are enforced.
</enforcement>