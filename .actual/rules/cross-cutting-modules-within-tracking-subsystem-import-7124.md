---
version: 2
name: cross-cutting-modules-within-tracking-subsystem-import-7124
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: Modules Within Tracking Subsystem Import Shared"
metadata:
  actual:
    adr_id: 7124f197-a425-469d-8abe-c920e294c6f2
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: Modules Within Tracking Subsystem Import Shared

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRACK-001** MUST: Modules within the tracking subsystem MUST import shared helper logic and date evaluation routines from the internal utilities module rather than defining local redundant utility functions.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute the test runner to validate utility function behavior and consumer module regression suites.
# Inspect the project dependency manifest to locate the static analysis and linting scripts, then run them to confirm proper internal module import adherence across tracking components.
```

**Accept when:**
- All unit tests covering internal utility functions and their consumer modules pass without failures.
- Static analysis checks verify that tracking domain modules import common date helpers from the internal utilities module rather than defining duplicate local functions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>