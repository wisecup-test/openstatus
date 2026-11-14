---
version: 2
name: cross-cutting-domain-feature-modules-not-duplicate-66fc
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: Domain Feature Modules Not Duplicate Date"
metadata:
  actual:
    adr_id: 66fcca47-c77d-4410-bdaf-dfd93b05696c
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: Domain Feature Modules Not Duplicate Date

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRACK-001** MUST_NOT: Domain feature modules MUST NOT duplicate date arithmetic or parse raw date representations independently when corresponding helper capabilities exist in the internal utilities module.

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