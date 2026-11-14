---
version: 2
name: cross-cutting-functions-exported-internal-utilities-mo-c268
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: Functions Exported Internal Utilities Module Remain"
metadata:
  actual:
    adr_id: c2681279-a24e-4bed-a3ed-82d3bb35d084
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: Functions Exported Internal Utilities Module Remain

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** SHOULD: Functions exported by the internal utilities module SHOULD remain pure, deterministic, and free of side effects or stateful component dependencies.

### Verify

```bash
# Discover and execute project test runner via manifest
# Discover and execute static analysis/linting scripts via manifest
```

**Accept when:**
- All unit tests covering internal utility functions and their consumer modules pass without failures.
- Static analysis checks verify that tracking domain modules import common date helpers from the internal utilities module rather than defining duplicate local functions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>