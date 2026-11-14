---
version: 2
name: cross-cutting-temporal-validation-collection-matching-40b9
description: "Adoption of Internal Utilities Module ./utils for Shared Operations: Temporal Validation Collection Matching Logic Including"
metadata:
  actual:
    adr_id: 40b9917a-749d-47ee-8666-26503e8839fa
    status: accepted
    candidate_group_id: 607853f0-c585-4550-bd89-7cf890750abe
    evidence:
      source: deep
  scope:
    root: packages/tracker
    paths:
      - src/**
---

# Adoption of Internal Utilities Module ./utils for Shared Operations: Temporal Validation Collection Matching Logic Including

Scope: applies to `packages/tracker/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** MUST: Temporal validation and collection matching logic, including blacklist date evaluation, MUST invoke the isSameDay comparison function provided by the internal utilities module.

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